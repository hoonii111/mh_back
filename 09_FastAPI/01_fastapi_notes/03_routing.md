<!-- 라우팅 기본 및 경로 매개변수 -->
# 라우팅(routing)

## 1. FastAPI의 라우팅이란?

 - FastAPI에서 라우팅 = 클라이언트의 요청 URL 경로를 해당 함수(view)와 연결  
 - `@app.get()`, `@app.post()` 등 데코레이터를 사용해 HTTP 메서드별 경로  정의

---

## 2. 기본 라우팅 예시

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello, FastAPI!"}

@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id}
```

- `/` : 루트 경로에서 "Hello, FastAPI!" 반환  
- `/items/{item_id}` : 동적 경로 파라미터 사용

---

## 3. HTTP 메서드 데코레이터

| 데코레이터       | 설명              |
|------------------|-------------------|
| `@app.get()`     | GET 요청 처리     |
| `@app.post()`    | POST 요청 처리    |
| `@app.put()`     | PUT 요청 처리     |
| `@app.delete()`  | DELETE 요청 처리  |
| `@app.patch()`   | PATCH 요청 처리   |

---

## 4. 경로 매개변수 (Path Parameters)

 - URL의 일부분을 변수로 처리 가능  
 - 기본적으로 문자열로 처리되며 타입을 명시할 수 있음

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}
```

📌 위 함수는 `/users/5` 같은 요청에서 `user_id = 5`로 추출됨

---

## 5. 경로 매개변수 타입 지정

FastAPI는 Python의 타입 힌트를 사용해 자동 검증 및 변환을 제공

```python
@app.get("/items/{item_id}")
def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q}
```

- `item_id`는 정수형으로 자동 변환됨  
- `q`는 쿼리 파라미터 (옵션)

---

## 6. 라우터 분리 (APIRouter)

라우팅이 많아질 경우, 파일을 분리해서 모듈화할 수 있다.

### `routers/items.py`

```python
from fastapi import APIRouter

router = APIRouter()

@router.get("/items/")
def get_items():
    return [{"name": "Item1"}, {"name": "Item2"}]
```

### `main.py`

```python
from fastapi import FastAPI
from routers import items

app = FastAPI()
app.include_router(items.router, prefix="/api")
```

📌 `/api/items/` 경로로 접근 가능

---

## 7. 실행 명령어

```bash
uvicorn main:app --reload
```

---

## ✅ 정리

- FastAPI는 데코레이터 기반으로 직관적인 라우팅을 제공  
- Path 매개변수와 타입 힌트를 활용해 깔끔한 코드 작성 가능  
- APIRouter를 통해 라우팅 모듈화가 가능하여 유지보수에 용이함

---