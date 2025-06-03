<!-- Path, Query, Body 파라미터 -->
# Path/Query/Body 파라미터

## 1. 경로 파라미터 (Path Parameters)

- URL 경로 내에서 동적으로 값을 받음.
- 타입 힌트를 통해 자동 검증 가능

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id}
```

- `/items/10` 요청 시 `item_id = 10`
- 타입 오류가 있으면 자동으로 422 응답

---

## 2. 쿼리 파라미터 (Query Parameters)

- `?key=value` 형식으로 URL 뒤에 붙는 파라미터
- 기본값 설정으로 선택적 파라미터 정의 가능

```python
@app.get("/search")
def search_items(keyword: str = "", limit: int = 10):
    return {"keyword": keyword, "limit": limit}
```

- `/search?keyword=fastapi&limit=5`

---

## 3. 필수 vs 선택 쿼리 파라미터

- 기본값이 없으면 필수 파라미터
- `Optional` 또는 `= None`으로 선택적 설정 가능

```python
from typing import Optional

@app.get("/users")
def get_user(name: Optional[str] = None):
    return {"name": name or "anonymous"}
```

---

## 4. 요청 본문 - Body Parameters

- 주로 `POST`, `PUT` 요청에서 사용
- `pydantic.BaseModel`을 사용해 JSON 데이터를 자동 검증

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float

@app.post("/items/")
def create_item(item: Item):
    return {"item": item}
```

요청 본문 예시:

```json
{
  "name": "book",
  "price": 12.99
}
```

---

## 5. 여러 파라미터 조합

- 경로 + 쿼리 + 바디를 동시에 받을 수 있음

```python
@app.put("/items/{item_id}")
def update_item(item_id: int, q: Optional[str] = None, item: Item = Body(...)):
    return {"item_id": item_id, "q": q, "item": item}
```

---

## 6. Body에 대한 설명 및 예시 추가

- `Body(..., example=...)`로 요청 예시 제공

```python
from fastapi import Body

@app.post("/example")
def with_example(data: dict = Body(..., example={"key": "value"})):
    return data
```

---

## ✅ 7. 정리

- 경로 파라미터: URL 경로의 일부
- 쿼리 파라미터: `?key=value` 형식의 값
- 바디 파라미터: JSON 본문, `BaseModel`을 통해 자동 검증
- 조합 사용 가능, 예시도 지정 가능

---