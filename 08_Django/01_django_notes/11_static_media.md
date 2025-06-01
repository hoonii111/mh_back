<!-- 정적 파일과 미디어 파일 관리 -->
# 정적 파일 (Static Files) & 미디어 파일 (Media Files)

## 1. 개요  
Django에서는 CSS, JS, 이미지 같은 **정적 파일(static files)**과 사용자 업로드 파일인 **미디어 파일(media files)**을 분리하여 관리  
각각의 파일들은 설정된 디렉토리에 저장되고, URL을 통해 제공됨.

---

## 2. 정적 파일 (Static Files)

### 2.1 static 파일 위치

- 각 앱의 `static/` 디렉토리에 파일 저장 가능  
- 전역(staticfiles_dirs) 경로도 설정 가능

```
myapp/
├── static/
│   └── myapp/
│       └── style.css
```

### 2.2 settings.py 설정

```python
# 정적 파일 URL
STATIC_URL = '/static/'

# 개발 환경에서 정적 파일 추가 위치
STATICFILES_DIRS = [
    BASE_DIR / 'static',
]

# collectstatic으로 모을 위치 (배포용)
STATIC_ROOT = BASE_DIR / 'staticfiles'
```

### 2.3 템플릿에서 static 파일 불러오기

```html
{% load static %}
<link rel="stylesheet" href="{% static 'myapp/style.css' %}">
```

> ⚠ `load static`은 반드시 템플릿 상단에 있어야 함

---

## 2. 미디어 파일 (Media Files)

### 3.1 사용자 업로드 파일 관리

예: 프로필 사진, 게시글 첨부 이미지 등

### 3.2 settings.py 설정

```python
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```

### 3.3 모델에서 파일 필드 사용

```python
from django.db import models

class Profile(models.Model):
    username = models.CharField(max_length=100)
    photo = models.ImageField(upload_to='profiles/')
```

### 3.4 개발 서버에서 media 파일 제공

`urls.py`에 아래와 같이 설정 (개발 환경에서만 사용)

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # ... 기존 URLConf
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

---

## 4. collectstatic (배포 시)

```bash
python manage.py collectstatic
```

- 모든 static 파일을 `STATIC_ROOT`에 모아 배포 준비
- 일반적으로 배포 서버에서 이 경로를 웹 서버가 제공함

---

## 5. 정리

| 항목         | 설명                                    |
|--------------|-----------------------------------------|
| STATIC_URL   | 정적 파일 URL prefix (`/static/`)       |
| STATICFILES_DIRS | 개발 중 사용자의 정적 파일 위치        |
| STATIC_ROOT  | 배포 시 `collectstatic`으로 모이는 경로 |
| MEDIA_URL    | 미디어 파일 URL prefix (`/media/`)      |
| MEDIA_ROOT   | 업로드 파일 저장 위치 (`media/`)        |
