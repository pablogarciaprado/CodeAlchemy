
◀️ [Home](../../../README.md)

## `requests`
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
   - status_code -> HTTP status code (e.g., 200 OK, 404 Not Found).
   - text -> Response body as a string.
   - json() -> Parses JSON response if the content is JSON.
   - headers -> Response headers.

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
1. Authentication -> Required for protected endpoints
```python
{"Authorization": "Bearer YOUR_ACCESS_TOKEN"}
```

2. Content-Type -> Defines the request body format
```python
{"Content-Type": "application/json"}  # JSON payload
{"Content-Type": "application/x-www-form-urlencoded"}  # Form data
```

3. User-Agent -> Identifies the client making the request
```python
{"User-Agent": "Mozilla/5.0"}  # Spoof browser requests
```

4. Accept -> Tells the server what response format is expected
```python
{"Accept": "application/json"}
```
