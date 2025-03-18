◀️ [Home](../README.md)

# **Python**

## Control Flow Statements
### `break` – Exits the loop entirely
```python
for i in range(5):
    if i == 3:
        break  # Stops the loop when i == 3
    print(i)

# Output: 0, 1, 2
```
### `continue` – Skips the current iteration and moves to the next
```python
for i in range(5):
    if i == 3:
        continue  # Skips printing 3
    print(i)

# Output: 0, 1, 2, 4
```
### `pass` – Does nothing, used as a placeholder
```python
def my_function():
    pass  # Placeholder for future code

for i in range(5):
    if i == 2:
        pass  # Doesn't affect the loop
    print(i)

# Output: 0, 1, 2, 3, 4
```
### `else` in loops – Runs only if the loop does not exit via break
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
### `try-except-finally-else` – Handling exceptions
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

## Dictionary comprehension
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
2. Iterating through each dictionary’s key-value pairs
`for k, v in d.items()`: `.items()` returns the key-value pairs from each dictionary `d`. The loop extracts each key `k` and its corresponding value `v`.
3. Constructing a new dictionary
`{k: v for ...}`: This creates a new dictionary where: Each key `k` is taken from the nested loop. Each value `v` is taken from the nested loop.
If multiple dictionaries have the same key, the last one encountered in `value` will overwrite the previous one

## `Flask` library
### Blueprint
A Flask Blueprint is a way to organize and structure a Flask application by grouping related routes, views, and other logic. It helps break a large application into smaller, modular components, making it easier to manage and maintain.
- Structuring large applications by separating concerns (e.g., authentication, API routes, admin panel).
- Reusing code across multiple applications.
- Keeping routes and logic modular and maintainable.

```py
from flask import Blueprint

# Create a Blueprint instance
auth_bp = Blueprint('auth', __name__)

@auth_bp.route('/login')
def login():
    return "Login Page"

@auth_bp.route('/logout')
def logout():
    return "Logout Page"

# Register the Blueprint in the main app
from flask import Flask
app = Flask(__name__)
app.register_blueprint(auth_bp, url_prefix='/auth')

if __name__ == '__main__':
    app.run()
```

## Google Sheets API Setup
### 1. Install the required libraries
```bash
pip install gspread google-auth
```

### 2. Authenticate with your service account and open the Google Sheet
```python
import gspread
from google.oauth2.service_account import Credentials

# Define the scope
scope = ["https://spreadsheets.google.com/feeds", "https://www.googleapis.com/auth/drive"]

# Path to your service account JSON key
json_keyfile_path = "/Users/your_name/your_repo_name/you_auth_folder/your-service-account.json"

# Authenticate using service account credentials
credentials = Credentials.from_service_account_file(json_keyfile_path, scopes=scope)
client = gspread.authorize(credentials)

# Open the Google Spreadsheet by key
spreadsheet = client.open_by_key("your_spreadsheet_key")

# Select the first worksheet by its ID
worksheet = spreadsheet.get_worksheet_by_id(0)
```

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

## `requests` library
`requests.get()` is used to send an HTTP GET request to a specified URL and retrieve the response.

```py
import requests

response = requests.get("https://api.github.com")

print(response.status_code)  # HTTP status code (e.g., 200, 404)
print(response.text)         # Response body as a string
print(response.json())       # Parse JSON response (if applicable)

```

1. Sends an HTTP GET request to the specified URL.
2. Receives a response from the server.
3. Stores the response in a Response object, which includes:
    - status_code → HTTP status code (e.g., 200 OK, 404 Not Found).
    - text → Response body as a string.
    - json() → Parses JSON response if the content is JSON.
    - headers → Response headers.

### `params`
You can pass query parameters using the params argument. Used to send data in the URL's query string. Commonly used in API requests to filter or modify responses and sent as key-value pairs.
> Use Cases: Pagination, filtering, search queries, API authentication (sometimes).
```py
# Example: Fetching paginated data
import requests

url = "https://api.example.com/items"
params = {"page": 2, "limit": 10}  # Query parameters

response = requests.get(url, params=params)
print(response.url)  # Shows: https://api.example.com/items?page=2&limit=10
```

### `headers`
Used to send additional metadata with the request. Typically includes authentication tokens, content types, and custom headers.
> Use Cases: Authentication, setting content types (JSON, XML), customizing requests.
```py
# Example: Sending an API key in the headers
headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Accept": "application/json"
}

response = requests.get("https://api.example.com/protected", headers=headers)

```
Headers follow a general structure, but the specific headers required depend on the website or API.

**Common Headers (Used Across Most APIs/Websites):**
1. Authentication → Required for protected endpoints

```python
{"Authorization": "Bearer YOUR_ACCESS_TOKEN"}
```
2. Content-Type → Defines the request body format
```python
{"Content-Type": "application/json"}  # JSON payload
{"Content-Type": "application/x-www-form-urlencoded"}  # Form data
```

3. User-Agent → Identifies the client making the request
```python
{"User-Agent": "Mozilla/5.0"}  # Spoof browser requests
```

4. Accept → Tells the server what response format is expected
```python
{"Accept": "application/json"}
```

## Script Execution Control
### `__name__ == "__main__"`

```python
if __name__ == "__main__":
	# the desired code
```
> If the script is being run directly (for example, by typing `python script.py` in the terminal), then `__name__` is set to `"__main__"` automatically. If the script is being imported into another script, `__name__` is set to the name of the script/module (e.g., "script").
