<!-- FastAPI 프로젝트 구조 및 실행 방법 -->
# FastAPI 프로젝트 세팅

## 1. 가상환경 생성

```bash
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
```

## 2. FastAPI & Uvicorn 설치

```bash
pip install fastapi
pip install "uvicorn[standard]"
```

## 3. 폴더 구조 예시

```
fastapi_app/
├── app/
│   ├── main.py
│   ├── routers/
│   │   └── __init__.py
│   ├── models/
│   │   └── __init__.py
│   ├── schemas/
│   │   └── __init__.py
│   ├── services/
│   │   └── __init__.py
│   └── core/
│       └── __init__.py
├── requirements.txt
└── README.md
```

## 4. `main.py` 기본 구조

```python
# app/main.py
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello, FastAPI"}
```

## 5. 실행 명령어

```bash
uvicorn app.main:app --reload
```

- `app.main`: 경로 (`app/main.py`)
- `app`: FastAPI 인스턴스 변수
- `--reload`: 코드 변경 시 자동 반영

## 6. requirements.txt 저장

```bash
pip freeze > requirements.txt
```

## 7. 자동 재시작을 위한 디렉토리 구조 주의

- `uvicorn` 실행 시 `main.py` 경로가 올바르게 지정되어야 함
- `app.main:app` 형태로 지정

---