<!-- Django REST Framework 기초 -->
# Django Framework 구조 및 작동 원리

## 1. Django 프레임워크란?

- Django = Python 기반의 **풀스택 웹 프레임워크**
- MTV(Model–Template–View) 아키텍처를 따름
- 장고는 빠른 개발과 보안, 유지 보수를 쉽게 하기 위해 많은 기능을 기본 제공

---

## 2. MTV 패턴

| 구성 요소 | 설명 |
|-----------|------|
| Model     | 데이터베이스 구조 및 ORM 관리 |
| Template  | 사용자에게 보여지는 HTML 출력 |
| View      | 비즈니스 로직 처리 및 응답 반환 |

> Django에서는 **View가 Controller의 역할**도 수행함

---

## 3. 요청(Request) 흐름

```plaintext
브라우저 요청
   ↓
urls.py → View → Model → Template
   ↓
HTTP 응답
```

---

## 4. 구성 요소별 설명

### 4.1 URL Dispatcher (`urls.py`)

- 클라이언트 요청 URL을 view 함수로 라우팅
```python
from django.urls import path
from . import views

urlpatterns = [
    path('hello/', views.hello_world),
]
```

---

### 4.2 View (`views.py`)

- 로직 처리 및 모델, 템플릿 연결
```python
from django.shortcuts import render

def hello_world(request):
    return render(request, 'hello.html', {'message': 'Hello, Django!'})
```

---

### 4.3 Model (`models.py`)

- DB 테이블과 매핑되는 클래스
```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
```

---

### 4.4 Template (`templates/`)

- HTML 파일로 사용자에게 보여질 화면 작성
- Django Template Language(DTL) 사용
```html
<h1>{{ message }}</h1>
```

---

## 5. Django 주요 기능 요약

| 기능 | 설명 |
|------|------|
| ORM | SQL 없이 DB 조작 가능 |
| Admin | 자동 생성되는 관리자 페이지 |
| Forms | 폼 처리 및 유효성 검사 |
| Auth | 로그인/로그아웃, 사용자 인증 기능 내장 |
| Static/Media | 정적 파일, 업로드 파일 처리 |
| Middleware | 요청/응답 중간 처리 |

---

## 6. 프레임워크 계층도

```plaintext
[Browser]
   ↓
[urls.py] → [views.py] → [models.py] → [DB]
                        → [templates/] → [HTML]
```

---

## ✅ 정리

- Django는 MTV 아키텍처 기반의 웹 프레임워크
- URL → View → Model & Template 흐름으로 작동
- 개발자에게 많은 기능을 **자동화** 및 **추상화**해서 빠르고 안전한 웹 개발을 지원
