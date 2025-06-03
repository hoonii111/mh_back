<!-- FastAPI 개요, 특징, 설치 -->
# FastAPI Overview

## 1. 개요

- FastAPI는 Python 기반의 **비동기 웹 프레임워크(Starlette)**
- 빠르고, 현대적인(Modern), 직관적인 API 개발 가능
- Swagger, ReDoc 문서 자동 제공
- 데이터 검증/설정을 위해 **Pydantic** 라이브러리 사용
- Starlette 위에서 동작 → 비동기 처리 지원

## 2. 특징

- **비동기(Async) 지원**: `async def`로 비동기 I/O 처리
- **자동 문서화**: Swagger (인터랙티브), ReDoc (정적)
- **타입 힌트 기반**: 타입 힌트를 통해 유효성 검증, 문서 자동 생성
- **빠른 속도**: Uvicorn + Starlette 조합으로 빠름
- **Pydantic**을 사용한 데이터 직렬화/역직렬화
- **Dependency Injection(의존성 주입)** 내장 지원

## 3. 주요 비교

| 항목         | FastAPI            | Django              | Flask              |
|--------------|--------------------|----------------------|--------------------|
| 비동기 지원  | ✅ (기본 지원)     | ❌ (3.2부터 일부 지원) | ❌ (별도 설정 필요) |
| 문서 자동화  | ✅ (Swagger, ReDoc) | ❌ (서드파티 필요)     | ❌ (서드파티 필요)   |
| 속도         | 🚀 빠름             | 보통                  | 보통               |
| 타입 힌트    | ✅ (기본 기능)      | ❌                    | ❌                 |

## 4. 설치

```bash
pip install fastapi
pip install "uvicorn[standard]"
```

- `fastapi`: 프레임워크 본체
- `uvicorn`: ASGI 서버 (FastAPI는 ASGI 기반)

## 5. 실행 예시

```bash
uvicorn main:app --reload
```

- `main`: main.py 파일 이름
- `app`: FastAPI 인스턴스 이름
- `--reload`: 코드 변경 시 자동 재시작 (개발용)

## 6. 첫 예제

```python
# main.py
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello, FastAPI!"}
```

## 7. 문서 확인

FastAPI 실행 후 아래 주소 접속:

- Swagger UI: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
- ReDoc UI: [http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)

---
