# `from __future__ import annotations`

◀️ [Home](../../../README.md)

This import enables **postponed evaluation of annotations** (PEP 563). Type hints in that module are stored as unevaluated strings instead of being resolved when the module is imported.

```python
from __future__ import annotations

def embed_query(query_text: str) -> list[float]:
    ...
```

At import time, Python records `"list[float]"` rather than looking up `list` and building a generic alias immediately.

---

## What changes at runtime

**Without the future import**, annotations are evaluated eagerly:

```python
# genai.Client must already be imported and defined
_shared_vertex_client: genai.Client | None = None
```

**With the future import**, the annotation is deferred:

```python
from __future__ import annotations

_shared_vertex_client: genai.Client | None = None
# Stored internally as the string "genai.Client | None"
```

That affects import behavior, not business logic. Functions run the same; only how type hints are recorded changes.

---

## Why use it

### 1. Forward references without quotes

Reference a class before it is defined, or avoid string quoting:

```python
from __future__ import annotations

class Node:
    def parent(self) -> Node | None:  # works
        ...

# Without __future__, you would write:
# def parent(self) -> "Node | None":
```

### 2. Cleaner modern union syntax

`str | None` and `genai.Client | None` read naturally. On Python 3.10+ the `X | Y` syntax works either way, but postponed evaluation still avoids resolving those names at import time.

### 3. Fewer circular-import problems

If module A imports B and B's type hints mention something from A, eager evaluation can fail during import. Deferring evaluation keeps hints as strings until something (a type checker or `get_type_hints()`) resolves them.

### 4. Faster, lighter imports

Heavy or side-effectful types are not constructed just because they appear in a hint. Hints stay metadata for static analysis and IDEs.

---

## What it does *not* do

| Expectation | Reality |
|-------------|---------|
| Validates types at runtime | No — mypy, pyright, or Pydantic do that separately |
| Changes function behavior | No — only annotation storage |
| Makes invalid types valid at runtime | No — if you call `typing.get_type_hints()`, resolution can still fail |

To resolve deferred hints at runtime:

```python
import typing

typing.get_type_hints(embed_query)  # {"query_text": str, "return": list[float]}
```

---

## Common pattern in production code

Many services put the import at the top of every module that uses type hints:

```python
"""Vertex text embeddings for query-time retrieval."""

from __future__ import annotations

import threading
from typing import Sequence

from google import genai
```

Example from `intelligence-layer-chat` (`src/embed/vertex.py`, `src/config/settings.py`): the import is present even when hints are simple (`str`, `list[float]`) so all modules follow the same convention and stay safe if hints grow more complex later.

Pairing with cached settings is typical — hints describe config and clients; they are not executed on each request:

```python
from __future__ import annotations

from functools import lru_cache

@lru_cache(maxsize=1)
def get_settings() -> ChatSettings:
    return ChatSettings()
```

See also: [`functools.lru_cache`](lru_cache.md).

---

## Python version notes

| Version | Behavior |
|---------|----------|
| **3.7–3.10** | Opt in via `from __future__ import annotations` (PEP 563) |
| **3.10+** | `X \| Y` union syntax available natively |
| **3.11+** | Default annotation handling changed (PEP 649); annotations are stored for lazy evaluation without always stringifying |
| **3.13+** | PEP 563 postponed evaluation is deprecated as the long-term default path; the `__future__` import still works |

For libraries and apps targeting 3.10+, keeping the future import is still a widespread, explicit choice: consistent deferred string annotations across the codebase, regardless of minor version differences.

---

## When to use it

| Good fit | Optional / skip |
|----------|-----------------|
| New Python 3.10+ projects with type hints everywhere | Scripts with no type hints |
| Modules that may reference types defined later in the file | Code that relies on inspecting `__annotations__` as live objects at import time |
| Packages avoiding circular-import pain in hint-heavy code | Framework code that must match legacy eager-annotation semantics |

Place it **first among imports** (after the module docstring), before other imports:

```python
"""Module docstring."""

from __future__ import annotations

import os
```

Only one `__future__` import block is allowed, and it must come before any other code except comments and docstrings.

---

## Related

- [PEP 563 — Postponed Evaluation of Annotations](https://peps.python.org/pep-0563/)
- [PEP 649 — Deferred Evaluation of Annotations Using Descriptors](https://peps.python.org/pep-0649/)
- [`typing.get_type_hints`](https://docs.python.org/3/library/typing.html#typing.get_type_hints) — resolve deferred annotations at runtime
- [`functools.lru_cache`](lru_cache.md) — often used in the same modules (settings singletons, client factories)
