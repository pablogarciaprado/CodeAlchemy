◀️ [Home](../../README.md)

# `class`

## `super()`
`super()` is used inside a class—typically in the `__init__` method—to call a method from a parent (or superclass). It's most commonly used to extend the functionality of an inherited method without completely overriding it.

In the context of `__init__`, `super()` is used to:
- Call the `__init__` method of the parent class.
- Ensure proper initialization of the parent class, especially in inheritance hierarchies.

Example:
```python
class Animal:
    def __init__(self):
        print("Animal initialized")

class Dog(Animal):
    def __init__(self):
        super().__init__()  # Calls Animal's __init__
        print("Dog initialized")
```

Output:
```python
# Animal initialized
# Dog initialized
```
