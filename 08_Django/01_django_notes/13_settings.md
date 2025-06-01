<!-- settings.py 주요 설정과 관리 방법 -->
# settings.py 설정 정리

## 1. 개요

- `settings.py` = Django 프로젝트의 **전역 설정 파일**
- 프로젝트의 모든 동작(데이터베이스, 앱, 정적 파일, 시큐리티 등)을 정의

---

## 2. 주요 설정 항목

### BASE_DIR

- 프로젝트의 **최상위 경로**
```python
from pathlib import Path
BASE_DIR = Path(__file__).resolve().parent.parent
```

---

### SECRET_KEY

- Django 프로젝트의 **암호화 키**
- 절대 외부에 노출 X (배포 시 환경 변수로 관리 추천)

---

### DEBUG

- `True`: 개발 모드 (자세한 에러 메시지 표시)
- `False`: 배포 모드 (보안 강화됨)

---

### ALLOWED_HOSTS

- 배포 시 접근 가능한 호스트 지정
```python
ALLOWED_HOSTS = ['localhost', '127.0.0.1', '.mydomain.com']
```

---

## 3. INSTALLED_APPS

- 현재 프로젝트에 등록된 앱 목록
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    # ...
    'myapp',  # 사용자 정의 앱
]
```

---

## 4. MIDDLEWARE

- 요청/응답 사이 동작하는 로직 정의 (보안, 세션 등)
```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    # ...
]
```

---

## 5. TEMPLATES

- 템플릿(HTML) 설정 정의
```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],  # 커스텀 템플릿 디렉터리
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                # ...
            ],
        },
    },
]
```

---

## 6. DATABASES

- 데이터베이스 설정
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',  # 기본 SQLite
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

※ 실제 배포 시에는 PostgreSQL 등으로 교체 권장

---

## 7. AUTH_PASSWORD_VALIDATORS

- 비밀번호 유효성 검사 설정
```python
AUTH_PASSWORD_VALIDATORS = [
    {'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator'},
    # ...
]
```

---

## 8. LANGUAGE_CODE & TIME_ZONE

- 언어 및 시간대 설정
```python
LANGUAGE_CODE = 'ko-kr'
TIME_ZONE = 'Asia/Seoul'
USE_TZ = True
```

---

## 9. STATIC & MEDIA 설정

```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']  # 개발용

MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'  # 업로드 파일 저장
```

---

## 10. 기타 설정

### WSGI_APPLICATION
- 배포 시 사용되는 WSGI 설정

### DEFAULT_AUTO_FIELD
```python
DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
```
- 모델의 기본 기본 키 필드 타입 설정

---

## 11. 환경 분리 팁

- 개발(`dev`), 운영(`prod`) 설정을 나누기 위해 환경별 `settings` 모듈 관리 권장
- 예시:
```
myproject/
├── settings/
│   ├── __init__.py
│   ├── base.py
│   ├── dev.py
│   └── prod.py
```

---

## ✅ 정리

| 항목              | 설명                         |
|-------------------|------------------------------|
| `BASE_DIR`        | 프로젝트 루트 경로            |
| `INSTALLED_APPS`  | 사용 중인 앱 목록             |
| `DATABASES`       | DB 설정                       |
| `TEMPLATES`       | HTML 렌더링 템플릿 설정       |
| `STATIC_URL` 등   | 정적/미디어 파일 경로 설정     |
| `MIDDLEWARE`      | 요청/응답 처리 미들웨어 목록   |
| `LANGUAGE_CODE`   | 언어 설정 (`ko-kr` 등)        |
| `TIME_ZONE`       | 시간대 설정 (`Asia/Seoul`)    |
