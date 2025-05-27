<!-- 인증 시스템(로그인, 로그아웃, 사용자 모델) -->
# Django Authentication

## 1. 개요

- Django는 인증 시스템 기본 내장함
- 사용자 로그인/로그아웃, 권한 관리, 사용자 모델 제공함

---

## 2. 기본 인증 흐름

- 인증 기능은 `django.contrib.auth` 앱에서 제공함
- 기본 User 모델은 `AbstractUser` 상속 구조임
- 로그인 상태 확인: `request.user.is_authenticated`로 확인함

---

## 3. 로그인 처리

### URL 설정

```python
from django.contrib.auth import views as auth_views

urlpatterns = [
    path('login/', auth_views.LoginView.as_view(), name='login'),
]
```

### 템플릿 예시
 - registration/login.html

```html
<form method="post">
  {% csrf_token %}
  {{ form.as_p }}
  <button type="submit">로그인</button>
</form>
```

 - 로그인 후 기본 리다이렉트 URL은 /accounts/projile/임
 - **settings.py**에서 변경 가능

```python
LOGIN_REDIRECT_URL = '/'
```

---

## 4. 로그아웃 처리

```python
from django.contrib.auth.views import LogoutView

urlpatterns = [
    path('logout/', LogoutView.as_view(), name='logout'),
]
```

 - 로그아웃 후 리다이렉트 URL 설정 가능

```python
LOGOUT_REDIRECT_URL = '/'
```

---

## 5. 로그인 상태 확인 및 제한

### 템플릿에서 조건 처리

```html
{% if user.is_authenticated %}
  {{ user.username }}님 환영함
  <a href="{% url 'logout' %}">로그아웃</a>
{% else %}
  <a href="{% url 'login' %}">로그인</a>
{% endif %}
```

### 뷰 접근 제한

```python
from django.contrib.auth.decorators import login_required

@login_required
def dashboard(request):
    ...
```
 - 클래스 기반 뷰에서 제한하려면 **LoginRequiredMixin** 사용

```python
from django.contrib.auth.mixins import LoginRequiredMixin

class ProfileView(LoginRequiredMixin, View):
    ...
```

---

## 6. 사용자 생성

### 관리자에서 직접 생성 가능

 - **/admin/** 페이지에서 User 추가 가능

### 코드로 생성

```python
from django.contrib.auth.models import User

user = User.objects.create_user(username='testuser', password='testpass')
```

---

## 7. 사용자 모델 커스터마이징

 - 커스텀 User 모델 사용하려면 프로젝트 초기 단계에서 설정해야함.

```python
# accounts/models.py
from django.contrib.auth.models import AbstractUser

class CustomUser(AbstractUser):
    nickname = models.CharField(max_length=50)

# settings.py
AUTH_USER_MODEL = 'accounts.CustomUser'
```

 - **AbstracBaseUser** 상속 시 더 자유롭게 구현 가능 (but 복잡)

---

## 8. 비밀번호 관리

### 변경

```python
from django.contrib.auth.views import PasswordChangeView

urlpatterns = [
    path('password/change/', PasswordChangeView.as_view(), name='password_change'),
]
```

### 초기화(Reset)

```python
from django.contrib.auth import views as auth_views

urlpatterns = [
    path('password/reset/', auth_views.PasswordResetView.as_view(), name='password_reset'),
    ...
]
```

 - 이메일 전송 설정 필요(**EMAIL_BACKED**, **EMAIL_HOST** 등)

---

## 9. 권한 및 그룸

 - **is_staff**, **is_superuser**, **is_active** 속성 존재
 - **Permission** 모델로 세부 권한 설정 가능
 - **Group** 모델로 사용자 그룹화 가능

```python
user.has_perm('app_name.permission_codename')
user.groups.add(group)
```

---

## 10. 요약

 - Django = 인증 시스템 내장 => 로그인/로그아웃, 사용자 생성, 권한 관리 등 지원
 - 로그인 제한 = **@login_required** or **LoginRequiredMixin**으로 처리
 - 사용자 모델 = 프로젝트 초기부터 커스터마이징 가능
 - 비밀번호 초기화, 변경, 이메일 전송 등 확장 기능도 기본 지원