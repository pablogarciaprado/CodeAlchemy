# `functools.lru_cache`

◀️ [Home](../../../README.md)

`lru_cache` is a decorator from the standard library module `functools`. It **memoizes** a function: the first time you call it with a given set of arguments, Python runs the function body and stores the return value; later calls with the same arguments return the cached value without re-running the body.

**LRU** stands for *least recently used*. When the cache is full (`maxsize` reached), the entry that has not been used for the longest time is evicted first to make room for new entries.

---

## Basic usage

```python
from functools import lru_cache

@lru_cache(maxsize=128)
def expensive(n: int) -> int:
    # ... heavy work ...
    return n * n
```

- **`maxsize`**: maximum number of distinct argument tuples to cache. Use `None` for an unbounded cache (use with care).
- **Hashable arguments only**: cache keys are the function arguments, so they must be hashable (e.g. no mutable lists as args unless you convert them to tuples).

Inspect the cache:

```python
expensive.cache_info()   # hits, misses, maxsize, currsize
expensive.cache_clear()  # drop all cached results
```

---

## Singleton settings pattern

A common production pattern is a zero-argument loader for app configuration. Environment variables and `.env` files are read once; every caller shares the same settings object.

```python
from functools import lru_cache
from pydantic_settings import BaseSettings

class ChatSettings(BaseSettings):
    gcp_project_id: str | None = None
    pghost: str = "127.0.0.1"
    # ...

@lru_cache(maxsize=1)
def get_settings() -> ChatSettings:
    return ChatSettings()
```

Why this works well:

1. **No arguments** — every call is the same cache key, so one `ChatSettings` instance is created per process.
2. **`maxsize=1`** — documents intent explicitly: only one cached value is ever needed.
3. **Cheap repeated access** — modules such as database clients or embedding clients can call `get_settings()` without re-parsing env on every import or request.

Example from `intelligence-layer-chat` (`src/config/settings.py`): `get_settings()` is used in retrieval and embedding code so `ChatSettings()` (Pydantic + `.env`) is built once and reused.

---

## When to use it

| Good fit | Poor fit |
|----------|----------|
| Pure functions with stable inputs | Functions whose result must change when env/config changes at runtime |
| Expensive I/O or parsing done once per process | Functions with unhashable or huge argument spaces |
| Settings / client factories with no per-call variation | Security-sensitive values you must re-read frequently |

---

## Caveats

**Environment changes after startup**  
If you change `os.environ` or a `.env` file after the first `get_settings()` call, the cached object still holds old values. Clear the cache in tests or reload paths:

```python
get_settings.cache_clear()
settings = get_settings()
```

**Tests**  
Pytest modules that need different env per test should call `cache_clear()` in fixtures (e.g. `autouse`) so tests do not leak configuration across cases.

**Not a distributed cache**  
`lru_cache` is per-process memory only. It does not share state across workers (e.g. multiple Gunicorn/Uvicorn workers each have their own cache — which is usually what you want for settings).

**Thread safety**  
In CPython, the cache lookup for a given key is safe for concurrent reads once populated. The settings object itself should still be treated as read-only after creation if multiple threads use it.

---

## Related

- [`from __future__ import annotations`](future_annotations.md) — often appears in the same modules as cached settings and client factories.
- [`functools.cache`](https://docs.python.org/3/library/functools.html#functools.cache) — Python 3.9+ shorthand for `@lru_cache(maxsize=None)` (unbounded).
- Pydantic `BaseSettings` — loads and validates config from env; pair with `lru_cache` on the factory function, not on the model class.
