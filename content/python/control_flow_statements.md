◀️ [Home](../../README.md)

# Control Flow Statements

## `break` - Exits the loop entirely
```python
for i in range(5):
    if i == 3:
        break  # Stops the loop when i == 3
    print(i)

# Output: 0, 1, 2
```

## `continue` - Skips the current iteration and moves to the next
```python
for i in range(5):
    if i == 3:
        continue  # Skips printing 3
    print(i)

# Output: 0, 1, 2, 4
```

## `pass` - Does nothing, used as a placeholder
```python
def my_function():
    pass  # Placeholder for future code

for i in range(5):
    if i == 2:
        pass  # Doesn't affect the loop
    print(i)

# Output: 0, 1, 2, 3, 4
```

## `else` in loops - Runs only if the loop does not exit via break
```python
for i in range(3):
    print(i)
else:
    print("Loop finished without break")

# Output:
# 0
# 1
# 2
# Loop finished without break
```

## `try-except-finally-else` - Handling exceptions
```python
try:
    # Code that might raise an exception
except SomeError:
    # Handle the exception
else:
    # Runs only if no exception occurred
finally:
    # Always runs, whether an exception happened or not
```
