◀️ [Home](../../../../README.md)

# Flask

## Basic concepts

### 1) What Flask is (and is not)

- Flask is a lightweight Python web framework for building HTTP APIs and web apps.
- Flask maps URLs (routes) to Python functions (view functions).
- Flask is intentionally minimal: you add only what you need.
- Flask is **not** a frontend framework, full CMS, or batteries-included enterprise stack.

In a simple project's `app.py`:

- App is created with `app = Flask(__name__)`
- Routes are defined with `@app.route(...)`
- Responses are returned with `render_template`, `jsonify`, or `redirect`

---

### 2) Tiny web request diagram

```text
Browser (JS/HTML)
   |   HTTP request (GET /, POST /submit)
   v
Flask route in app.py
   |   calls business logic (ConfigStore/modules)
   v
Response (HTML / JSON / Redirect)
   |
   v
Browser renders UI or handles JSON
```

Mental model:

1. Client sends request.
2. Flask picks matching route + method.
3. Python code runs.
4. Flask returns response.

---

### 3) JavaScript basics (only what you need for Flask)

- JavaScript in the browser calls Flask endpoints (usually with `fetch`).
- For `POST` endpoints, JS often sends JSON payloads.
- Flask reads JSON with `request.get_json()` (or `request.json`).
- Flask returns JSON with `jsonify(...)`; JS updates UI based on response.

---

### 4) Basic Flask app structure

Typical shape:

1. Imports
2. App creation and config
3. Routes/endpoints
4. Local run block (`if __name__ == "__main__":`)

In a simple `app.py`:

- Imports Flask primitives: `Flask`, `request`, `render_template`, `jsonify`, `redirect`, `url_for`, `session`
- Initializes app
- Defines endpoints (e.g. `/login`, `/logout`, `/auth_callback`, `/`, `/submit`)
- Runs development server at the bottom

---

### 5) Routing and endpoints

- A **route** is a URL pattern (example: `"/submit"`).
- An **endpoint** is the Python function handling that route.
- HTTP method controls intent:
  - `GET` = retrieve page/data
  - `POST` = submit/change data

Example patterns for `app.py`:

- `@app.route("/login")` -> return login HTML
- `@app.route("/auth_callback", methods=["POST"])` -> process auth token
- `@app.route("/submit", methods=["POST"])` -> accept form submission JSON

---

### 6) Requests and responses

Common request sources:

- Query params: `request.args.get("message")`
- JSON body: `request.get_json()` or `request.json`
- Session data: `session["user_email"]`, `session["next_url"]`

Common response types:

- HTML: `render_template("index.html", ...)`
- JSON: `jsonify({"status": "success"})`
- Redirect: `redirect(url_for("login"))`

Session example in this app:

- On login callback, user identity and session cookie are stored in Flask session.
- On logout, `session.clear()` resets user state.

---

### 7) Templates (Jinja2)

- Flask uses Jinja2 templates for server-rendered HTML.
- Route handlers prepare data, then pass it to templates:
  - `render_template("index.html", clients=..., employees=..., user_email=...)`
- In template files, Jinja renders variables/loops/conditionals into HTML.

Rule of thumb:

- Python route = data and control flow
- Jinja template = presentation

---

### 8) Project structure cheat sheet

Example repo:

- `app.py` -> HTTP entrypoint and route definitions
- `templates/` -> Jinja2 HTML templates
- `static/` -> CSS/JS/images served to browser
- `modules/` -> business logic and integrations (auth handlers, processors, etc.)
- `config/` -> app configuration and object wiring (`ConfigStore`)

Why this matters:

- Keep request handling thin in routes.
- Push reusable logic to modules/services.

---

### 9) Glossary

- **Flask**: Python micro web framework.
- **Route**: URL pattern mapped to a function.
- **Endpoint**: The handler function for a route.
- **HTTP method**: Request type such as `GET` or `POST`.
- **Request**: Data sent by client to server.
- **Response**: Data sent by server to client.
- **JSON**: Structured data format used by APIs.
- **Template (Jinja2)**: HTML with dynamic placeholders and logic.
- **Session**: Server-managed user state across requests.
- **Decorator**: Wrapper that augments function behavior (auth, logging, etc.).
- **Redirect**: Response instructing browser to navigate to another URL.
- **`url_for`**: Flask helper to build URLs from endpoint names.

---

## Blueprint

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