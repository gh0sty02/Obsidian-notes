---
title: FastAPI – Python API Development
tags: [fastapi, python, async, pydantic, uvicorn, api]
sources: [sources/fastapi-study-plan.md]
last-updated: 2026-04-17
---

# FastAPI – Python API Development

## What is FastAPI?

FastAPI is a modern Python web framework for building APIs with automatic OpenAPI documentation, type validation via Pydantic, and native async support via ASGI (Starlette under the hood).

**Strengths:**
- Type hints → automatic validation + Swagger UI / ReDoc at `/docs` and `/redoc`
- Async-first — high concurrency with `async def` endpoints
- Dependency injection system — clean, testable architecture
- Fast: comparable to Node.js and Go for I/O-bound tasks

## Fundamentals

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float
    in_stock: bool = True

@app.post("/items/", response_model=Item)
async def create_item(item: Item):
    return item
```

Pydantic automatically validates the request body; FastAPI generates OpenAPI schema.

## Dependency Injection

```python
from fastapi import Depends

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

@app.get("/users/{user_id}")
async def get_user(user_id: int, db: Session = Depends(get_db)):
    return db.query(User).get(user_id)
```

Dependencies can be nested, shared, overridden in tests.

## Async Handling

```python
# Async endpoint — runs on event loop, non-blocking
@app.get("/data")
async def get_data():
    result = await some_async_db_call()
    return result

# Sync endpoint — FastAPI runs in a thread pool automatically
@app.get("/sync")
def sync_endpoint():
    return heavy_computation()  # doesn't block event loop
```

> [!warning] Blocking calls in async endpoints
> Never call blocking I/O (requests.get, time.sleep) inside `async def`. Use `asyncio.run_in_executor` or switch to `def`.

## Routing & Middleware

```python
from fastapi import APIRouter

router = APIRouter(prefix="/users", tags=["users"])

@router.get("/{id}")
async def get_user(id: int): ...

app.include_router(router)

# Middleware
@app.middleware("http")
async def log_requests(request: Request, call_next):
    start = time.time()
    response = await call_next(request)
    print(f"{request.method} {request.url} — {time.time()-start:.2f}s")
    return response
```

## Authentication

```python
from fastapi.security import OAuth2PasswordBearer
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

@app.get("/me")
async def get_me(token: str = Depends(oauth2_scheme)):
    user = verify_jwt(token)
    return user
```

JWT flow: `POST /token` → server returns `access_token` → client sends `Authorization: Bearer <token>`

## Database Integration

```python
# SQLAlchemy async
from sqlalchemy.ext.asyncio import AsyncSession

@app.get("/products")
async def list_products(db: AsyncSession = Depends(get_async_db)):
    result = await db.execute(select(Product))
    return result.scalars().all()
```

**ORM options:** SQLAlchemy (SQL, sync/async), Tortoise ORM (async), Motor (MongoDB async)

## Testing

```python
from fastapi.testclient import TestClient

client = TestClient(app)

def test_create_item():
    response = client.post("/items/", json={"name": "Widget", "price": 9.99})
    assert response.status_code == 200

# Override dependencies in tests
app.dependency_overrides[get_db] = lambda: mock_db
```

## Production Deployment

```bash
# Development
uvicorn main:app --reload

# Production: Gunicorn managing Uvicorn workers
gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker
```

**Docker:**
```dockerfile
FROM python:3.11-slim
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
CMD ["gunicorn", "main:app", "-w", "4", "-k", "uvicorn.workers.UvicornWorker"]
```

## Key Differences vs Flask/Django

| Feature | FastAPI | Flask | Django |
|---------|---------|-------|--------|
| Async | Native | Via extensions | Limited (3.1+) |
| Validation | Pydantic (automatic) | Manual | Forms/Serializers |
| Docs | Auto OpenAPI | Manual | DRF auto |
| Performance | High | Medium | Medium |
| ORM | Any | Any | Built-in |

## See Also

- [[concepts/microservices-patterns]] — FastAPI as microservice building block
- [[concepts/prompt-engineering]] — FastAPI commonly wraps LLM inference APIs
- [[concepts/react-fundamentals]] — FastAPI as React's backend
