# Print Full Traceback

```python
import traceback

def do_stuff():
    raise Exception("test exception")

try:
    do_stuff()
except Exception:
    print(traceback.format_exc())
```
