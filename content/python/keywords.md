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
