# `dataclass`

◀️ [Home](../../../README.md)

A `dataclass` is a class decorator (`@dataclass`) that automatically generates common methods like `__init__`, `__repr__`, and `__eq__` based on class fields.

## Why use it

- Reduces boilerplate for classes that mostly store data.
- Makes code more readable and easier to maintain.
- Supports defaults, type hints, ordering, immutability, and post-init logic.

> Use `dataclass` when your class is primarily structured data with minimal custom behavior.

## Basic example

```python
from dataclasses import dataclass

@dataclass
class User:
    name: str
    age: int = 0

user = User("Pablo", 24)
print(user)  # User(name='Pablo', age=24)
```

## Generated methods explained

- `__init__`: constructor. Creates the object and assigns field values.
  - Runs when you create an object.
  - Its job is to set up the object’s data (attributes).
  - Example call: `User("Pablo", 24)` sets `name` and `age`.
- `__repr__`: developer-friendly string representation of the object.
  - Runs when Python needs a developer-friendly text version of the object.
  - Example triggers: `print(obj)` (if no `__str__`), typing obj in REPL.
  - Example output: `User(name='Pablo', age=24)`.
- `__eq__`: equality comparison. Two instances are equal if all fields are equal.
  - Runs when you compare with `==`.
  - Example: `User("Ana", 20) == User("Ana", 20)` returns `True`.

## Useful options

- `frozen=True`: makes instances immutable.
- `order=True`: adds comparison methods (`<`, `<=`, etc.).
- `field(...)`: customize defaults, metadata, and behavior per field.
