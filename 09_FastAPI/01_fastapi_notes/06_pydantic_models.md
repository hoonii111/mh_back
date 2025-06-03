<!-- Pydantic을 이용한 데이터 검증 -->
# Pydantic

## 1. Pydantic이란?

- FastAPI는 데이터 검증 및 직렬화를 위해 [Pydantic](https://docs.pydantic.dev/) 사용
- `BaseModel`을 상속받아 데이터 모델 정의 가능.
- 타입 기반 자동 검증, 문서화 지원.

---

## 2. 기본 사용법

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float
    is_offer: bool = False
```

- `is_offer`는 선택적 필드 (기본값 있음)
- 타입이 맞지 않으면 자동으로 422 오류 반환

---

## 3. 경로에서 사용 예

```python
from fastapi import FastAPI

app = FastAPI()

@app.post("/items/")
def create_item(item: Item):
    return {"item": item}
```

요청 예시:

```json
{
  "name": "Book",
  "price": 10.5
}
```

---

## 4. 필드 유효성 검사

- `Field()`를 사용해 유효성 검사 조건 지정 가능

```python
from pydantic import BaseModel, Field

class User(BaseModel):
    name: str = Field(..., min_length=3)
    age: int = Field(..., gt=0)
```

---

## 5. 응답 모델 지정

- `response_model` 파라미터로 반환 타입 제한

```python
class ResponseItem(BaseModel):
    name: str
    price: float

@app.get("/items/{item_id}", response_model=ResponseItem)
def read_item(item_id: int):
    return {"name": "Phone", "price": 999.99, "extra_field": "ignored"}
```

- 응답 모델에 정의되지 않은 필드는 자동 제거됨

---

## 6. 중첩 모델

- 모델 안에 다른 모델을 포함할 수 있음

```python
class Address(BaseModel):
    city: str
    zipcode: str

class User(BaseModel):
    name: str
    address: Address
```

요청 예시:

```json
{
  "name": "Alice",
  "address": {
    "city": "Seoul",
    "zipcode": "12345"
  }
}
```

---

## 7. 리스트 및 딕셔너리 처리

```python
from typing import List, Dict

class TagsModel(BaseModel):
    tags: List[str]

class ScoresModel(BaseModel):
    scores: Dict[str, int]
```

---

## ✅ 8. 정리

- `BaseModel`을 상속받아 입력 데이터 구조 정의
- 타입 자동 검증 및 문서화
- `Field()`를 통한 유효성 검사
- 중첩, 리스트, 딕셔너리 등 다양한 타입 지원
- `response_model`로 출력 데이터 형식 제한

---