<!-- 요청/응답 처리 및 Request 객체 -->
# 요청/응답 처리

## 1. 요청과 응답 처리란?

 - FastAPI에서 **클라이언트의 요청(Request)** 을 받고 **서버의 응답(Response)** 을 반환하는 과정 = 핵심 로직
 - 요청 정보를 `Request` 객체로 받고, 응답을 `Response` 객체 또는 Python 데이터로 반환

---

## 2. Request 객체 사용

FastAPI = Starlette 기반이므로 `starlette.requests.Request` 를 사용

```python
from fastapi import FastAPI, Request

app = FastAPI()

@app.post("/log")
async def log_request(request: Request):
    body = await request.body()
    return {"method": request.method, "body": body.decode()}
```

- `request.method`: HTTP 메서드 (GET, POST 등)
- `await request.body()`: 요청 본문(body) 전체 가져오기 (비동기)

---

## 3. Response 객체 사용

응답 객체를 사용하면 상태 코드, 헤더, 쿠키 등 응답 세부 설정 가능.

```python
from fastapi import FastAPI
from fastapi.responses import JSONResponse

app = FastAPI()

@app.get("/custom")
def custom_response():
    return JSONResponse(
        status_code=201,
        content={"message": "Created"},
        headers={"X-Custom-Header": "FastAPI"}
    )
```

---

## 4. Response 클래스 종류

| 클래스                       | 설명                          |
|-----------------------------|-------------------------------|
| `JSONResponse`              | JSON 형식 응답 (기본값)       |
| `HTMLResponse`              | HTML 문자열 응답              |
| `PlainTextResponse`         | 일반 텍스트 응답              |
| `RedirectResponse`          | 다른 URL로 리다이렉트         |
| `FileResponse`              | 파일 다운로드 응답            |
| `StreamingResponse`         | 스트리밍 응답 (대용량 등)     |

---

## 5. 상태 코드 지정

`return`에 `Response` 객체를 쓰지 않더라도, 함수 데코레이터에 상태 코드를 지정 가능.

```python
from fastapi import FastAPI, status

app = FastAPI()

@app.post("/create", status_code=status.HTTP_201_CREATED)
def create_item():
    return {"message": "Item created"}
```

---

## 6. 헤더 및 쿠키 처리

```python
from fastapi import FastAPI, Response

app = FastAPI()

@app.get("/set-cookie")
def set_cookie(response: Response):
    response.set_cookie(key="session_id", value="abc123")
    return {"message": "Cookie set"}
```

- `response.set_cookie()`를 사용해 쿠키 설정 가능

---

## ✅ 7. 정리

- `Request` 객체로 요청 데이터 접근  
- `Response` 객체 또는 다양한 응답 클래스로 응답 구성  
- 응답 코드, 헤더, 쿠키 등을 유연하게 설정 가능  
- FastAPI는 비동기 방식 요청/응답 처리에 최적화되어 있음

---