◀️ [Home](../../README.md)

# Keywords

## `yield`

Very powerful and memory-efficient technique, especially for an application that handles large video files.

In Python, when you use the `yield` keyword in a function, you're not creating a regular function; you're creating a **generator function**.

Here's the difference between a regular function and a generator function:

- A regular function runs from start to finish and then returns a single value (or nothing). It computes everything at once.
- A generator function doesn't run all at once. It returns a special iterator called a generator. When you loop over this generator, the function's code executes only until it hits a yield statement. It then "yields" (sends back) a value and pauses its execution, saving its state (all its local variables). When you ask for the next item, it resumes right where it left off. Think of it like a "pause and resume" button for your function.

## Script Execution Control

### `__name__ == "__main__"`

```python
if __name__ == "__main__":
# the desired code
```

If the script is being run directly (for example, by typing `python script.py` in the terminal), then `__name__` is set to `"__main__"` automatically. If the script is being imported into another script, `__name__` is set to the name of the script/module (e.g., "script").

## `*` — keyword-only parameter separator

In a function definition, a bare `*` (not `*args`) marks the boundary between parameters that can be passed **positionally** and parameters that must be passed **by keyword**. Everything after `*` is keyword-only: callers must use `name=value`.

```python
def connect(host, port, *, timeout=30, retries=3):
    ...

connect("localhost", 8080)                    # OK — host and port are positional
connect("localhost", 8080, timeout=60)        # OK
connect("localhost", 8080, 60)                # TypeError — timeout is keyword-only
connect(host="localhost", port=8080, retries=5)  # OK — all keyword args
```

### Why use it?

- **Clearer call sites** — Boolean flags and optional settings read better as `save(path, overwrite=True)` than as a positional `True` whose meaning is unclear.
- **Safer APIs** — You can add new keyword-only parameters later without breaking callers who already pass earlier options by name.
- **Fewer mistakes** — Prevents accidentally swapping two optional arguments of the same type (e.g. two `int`s or two `str`s).

### `*` with no name (not the same as `*args`)

`*args` collects extra positional arguments into a tuple. A lone `*` does not bind a name; it only enforces the keyword-only rule for what follows:

```python
def log(message, *args, level="info"):
    # message: positional (or keyword)
    # *args:   extra positional values → tuple
    # level:   keyword-only
    ...

log("started", "a", "b", level="debug")  # OK
log("started", "a", "b", "debug")        # TypeError — "debug" cannot fill level positionally
```

If you do not need `*args`, you can put `*` alone to keyword-only everything after the required positional parameters:

```python
def create_user(name, email, *, role="member", active=True):
    ...
```

### Positional-only vs keyword-only (`/` and `*`)

Since Python 3.8, `/` marks parameters that must be passed positionally (positional-only), and `*` marks keyword-only parameters. Together they pin down exactly how each argument may be supplied:

```python
def divide(a, b, /, *, round_result=False):
    result = a / b
    return round(result) if round_result else result

divide(10, 2)                          # OK
divide(10, 2, round_result=True)     # OK
divide(a=10, b=2)                      # TypeError — a and b are positional-only
divide(10, 2, True)                    # TypeError — round_result is keyword-only
```

Order in the signature is: **positional-only** → **positional or keyword** → **keyword-only**.

### Defaults and required keyword-only parameters

Keyword-only parameters can be required (no default) or optional (with a default), same as any other parameter:

```python
def fetch(url, *, headers, timeout=10):
    # headers: required keyword-only
    # timeout: optional keyword-only
    ...

fetch("https://example.com", headers={"Accept": "application/json"})
fetch("https://example.com")  # TypeError — missing required keyword argument 'headers'
```

### Practical pattern

Libraries often use `*` for configuration knobs that should never be passed positionally:

```python
def read_csv(path, *, encoding="utf-8", delimiter=",", skip_rows=0):
    ...
```

Callers must write `read_csv("data.csv", delimiter=";")`, which makes the intent obvious at a glance and leaves room to add more keyword-only options (`on_bad_lines=`, `dtype=`, etc.) without ambiguous positional ordering.
