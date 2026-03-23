# FastAPI: Useful Concise Guide

FastAPI is a modern Python web framework for building APIs quickly with:

- Type hints for validation and editor support
- Automatic OpenAPI/Swagger docs
- High performance on ASGI (`uvicorn`)

---

## 1) Minimal app

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello FastAPI"}
```

Run:

```bash
uvicorn main:app --reload
```

Docs:

- Swagger UI: `http://127.0.0.1:8000/docs`
- ReDoc: `http://127.0.0.1:8000/redoc`

---

## 2) Path params, query params, and validation

```python
from fastapi import FastAPI, Query

app = FastAPI()

@app.get("/items/{item_id}")
def get_item(item_id: int, q: str | None = Query(default=None, min_length=2)):
    return {"item_id": item_id, "q": q}
```

- `item_id: int` is validated from the URL path.
- `q` is a query param with constraints (`min_length=2`).

---

## 3) Request body with Pydantic

```python
from pydantic import BaseModel
from fastapi import FastAPI

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float
    in_stock: bool = True

@app.post("/items")
def create_item(item: Item):
    return {"received": item.model_dump()}
```

FastAPI validates JSON input and returns clear errors automatically.

---

## 4) Async vs sync endpoints

```python
@app.get("/sync")
def sync_handler():
    return {"mode": "sync"}

@app.get("/async")
async def async_handler():
    return {"mode": "async"}
```

- Use `async def` when calling async libraries (HTTP clients, DB drivers, etc.).
- `def` is fine for CPU-bound or blocking code (consider workers/background tasks as needed).

---

## 5) Dependency Injection (`Depends`)

```python
from fastapi import Depends, FastAPI, Header, HTTPException

app = FastAPI()

def verify_token(x_token: str = Header(...)):
    if x_token != "secret-token":
        raise HTTPException(status_code=401, detail="Invalid token")
    return x_token

@app.get("/protected")
def protected_route(token: str = Depends(verify_token)):
    return {"ok": True}
```

`Depends` is the clean way to share auth, DB sessions, config, and reusable logic.

---

## 6) Lifespan events (startup + shutdown)

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI

@asynccontextmanager
async def lifespan(app: FastAPI):
    # startup
    app.state.cache = {"ready": True}
    yield
    # shutdown
    app.state.cache.clear()

app = FastAPI(lifespan=lifespan)
```

`@asynccontextmanager + async def lifespan(app: FastAPI):` declares a lifecycle context manager FastAPI runs at startup and shutdown.

- Code before `yield` runs on startup.
- Code after `yield` runs on shutdown.

If no shutdown logic is needed, you can leave the post-`yield` section empty.

>`@asynccontextmanager` turns an async def function with a yield into an asynchronous context manager. FastAPI expects a context manager for lifespan. `@asynccontextmanager` is what makes your `lifespan(...)` function valid for that contract. Without it, an async `def` + `yield` is just an async generator, not automatically usable as a context manager (`async with ...`).

---

## 7) Response models

```python
from pydantic import BaseModel
from fastapi import FastAPI

app = FastAPI()

class UserOut(BaseModel):
    id: int
    username: str

@app.get("/users/{user_id}", response_model=UserOut)
def get_user(user_id: int):
    return {"id": user_id, "username": "pablo", "password": "hidden"}
```

`response_model` filters output fields and documents the API contract.

---

## 8) Error handling

```python
from fastapi import HTTPException

@app.get("/orders/{order_id}")
def get_order(order_id: int):
    if order_id < 1:
        raise HTTPException(status_code=400, detail="Invalid order id")
    return {"order_id": order_id}
```

Use `HTTPException` for clear client-facing errors.

---

## 9) Project structure (simple and scalable)

```text
app/
  main.py
  routers/
    items.py
    users.py
  schemas/
    item.py
  services/
    item_service.py
```

Split routes, schemas, and business logic early to keep code maintainable.

---

## 10) FastAPI essentials checklist

- Type everything (params, bodies, return models)
- Prefer `response_model` for output safety
- Use `Depends` for shared concerns
- Use `lifespan` for startup/shutdown resources
- Keep endpoints thin; move logic to services

FastAPI is most powerful when you combine type hints + Pydantic models + clean dependency design.
