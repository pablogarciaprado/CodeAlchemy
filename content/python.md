# **Python**
## `__name__ == "__main__"`

```python
if __name__ == "__main__":
	# the desired code
```
> If the script is being run directly (for example, by typing `python script.py` in the terminal), then `__name__` is set to `"__main__"` automatically. If the script is being imported into another script, `__name__` is set to the name of the script/module (e.g., "script").

## Leaked semaphore objects
```bash
UserWarning: resource_tracker: There appear to be 1 leaked semaphore objects to clean up at shutdown
warnings.warn('resource_tracker: There appear to be %d '.
```

The warning is related to the use of multiprocessing or concurrent threads in Python, and it specifically points out that there are “leaked semaphore objects” that are not being cleaned up properly. The warning doesn’t necessarily point to a bug in the code, but it does indicate that the system is not managing resources like it should when working with concurrency. This can happen when resources are not released after use, which could be because threads or processes are not terminating as expected.

The part of the code where `ThreadPoolExecutor` is used could potentially be related to the problem, especially if it’s being used to process frames concurrently. It’s possible that the concurrent threads are not being cleaned up properly after execution, leading to resource leakage. 

With more threads or larger batches, the system may run out of available resources, which can lead to threads being orphaned or not properly cleaned up. Limiting the number of frames being processed in parallel at once should help. To process smaller chunks of frames, we can adjust the batch size or the number of workers in the `ThreadPoolExecutor`.

•	For **I/O-bound** tasks: `max_workers` can be higher than the number of CPU cores, because threads will often be idle waiting for I/O operations.

•	For **CPU-bound** tasks: It’s usually best to keep `max_workers` equal to the number of CPU cores to avoid overloading the CPU.

```python
os.cpu_count()  # Will return the number of physical cores
```

## Print Full Traceback
```python
import traceback
 
def do_stuff():
    raise Exception("test exception")
 
try:
    do_stuff()
except Exception:
    print(traceback.format_exc())
```

## `try-except` blocks

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