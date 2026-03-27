# Dictionary comprehension

A dictionary comprehension is a concise way to create dictionaries in Python using a single line of code. It follows the format:
```python
{key: value for key, value in iterable}
```

Let's look at a more complex example. This is a dictionary comprehension that merges multiple dictionaries into one while ensuring only the latest value is retained for each key.
```python
value = [{'windowboxing_orientation': 'letterboxing'},
         {'windowboxing_orientation': 'letterboxing'}]
unique_dicts = {k: v for d in value for k, v in d.items()}
```

1. Iterating through value (which is a list of dictionaries)
`for d in value`: This loops through each dictionary (`d`) inside the list `value`.

2. Iterating through each dictionary's key-value pairs
`for k, v in d.items()`: `.items()` returns the key-value pairs from each dictionary `d`. The loop extracts each key `k` and its corresponding value `v`.

3. Constructing a new dictionary
`{k: v for ...}`: This creates a new dictionary where: Each key `k` is taken from the nested loop. Each value `v` is taken from the nested loop.

If multiple dictionaries have the same key, the last one encountered in `value` will overwrite the previous one
