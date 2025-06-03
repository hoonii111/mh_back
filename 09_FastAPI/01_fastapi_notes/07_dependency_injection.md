<!-- 의존성 주입 -->
# 의존성 주입

## 1. 의존성 주입(Dependency Injection) 개요

- FastAPI = `Depends`를 통해 의존성 주입 기능 제공
- 공통 로직, 인증, DB 연결, 설정 값 등을 재사용 가능하게 함.
- 함수나 클래스를 인자로 삽입해 자동으로 실행 결과를 주입받음.

---

## 2. 기본 사용법

```python
from fastapi import Depends, FastAPI

app = FastAPI()

def common_dependency():
    return "공통 의존성"

@app.get("/items/")
def read_items(dep=Depends(common_dependency)):
    return {"message": dep}
```

- `common_dependency` 함수의 리턴값이 `dep`에 전달됨.

---

## 3. 의존성 주입의 실행 시점

- FastAPI = 요청마다 의존성을 **자동 호출**하고, 결과를 해당 파라미터에 주입함.
- 순서: 요청 → 의존성 실행 → 라우터 함수 실행

---

## 4. 클래스 기반 의존성

```python
class CommonParams:
    def __init__(self, q: str | None = None, limit: int = 10):
        self.q = q
        self.limit = limit

@app.get("/search/")
def search(params: CommonParams = Depends()):
    return {"q": params.q, "limit": params.limit}
```

- 생성자를 통해 파라미터 주입 가능
- 복잡한 설정이나 초기화에 적합

---

## 5. 의존성 계층화

- 의존성 안에서 다른 의존성을 또 사용할 수 있음

```python
def get_token():
    return "token"

def get_current_user(token: str = Depends(get_token)):
    return {"user": "admin", "token": token}

@app.get("/me")
def read_me(user=Depends(get_current_user)):
    return user
```

---

## 6. 의존성 재사용

- 공통 파라미터나 인증 로직을 여러 라우트에 반복해서 사용할 수 있음

```python
def common_query_params(q: str | None = None, page: int = 1):
    return {"q": q, "page": page}

@app.get("/posts/")
def read_posts(params=Depends(common_query_params)):
    return params
```

---

## 7. 의존성 주입과 상태 관리

- 객체를 싱글톤처럼 생성하고 유지하거나, 요청마다 새로 만들 수 있음
- 예: DB 세션, 설정 객체 등

```python
def get_settings():
    return {"debug": True}

@app.get("/config/")
def config(settings=Depends(get_settings)):
    return settings
```

---

## ✅ 8. 정리

- `Depends()`를 통해 의존성 함수를 주입할 수 있음
- 함수, 클래스 모두 의존성으로 사용 가능
- 의존성은 중첩 사용, 계층화, 재사용 가능
- 인증, 설정, DB 연결 등 다양한 공통 기능에 활용

---