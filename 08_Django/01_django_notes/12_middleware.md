<!-- 미들웨어 개념과 활용법 -->
# 미들웨어 (Middleware)

## 1. 개요

- **미들웨어(Middleware)** = Django의 요청(Request)과 응답(Response) 사이에서 작동하는 **처리 로직**
- 요청이 **뷰에 도달하기 전**과 응답이 **브라우저에 전달되기 전** 실행.  

---

## 2. 미들웨어의 역할

- 인증(Authentication)
- 세션(Session) 관리
- 메시지 처리
- 보안 처리 (예: CSRF, XSS)
- 사용자 정의 로깅/처리 등

---

## 3. 기본 구조

미들웨어 = **클래스 기반**으로 작성

```python
class SimpleMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        # 요청 전 처리 (뷰 함수 호출 전)
        print("Before view")

        response = self.get_response(request)

        # 응답 후 처리 (뷰 함수 호출 후)
        print("After view")
        return response
```

---

## 4. 미들웨어 등록

`settings.py`의 `MIDDLEWARE` 항목에 추가

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    # ... 생략 ...
    'myapp.middleware.SimpleMiddleware',  # 사용자 정의 미들웨어 추가
]
```

- 순서 중요: 위에서부터 순서대로 실행됨
  - 요청: 위 → 아래
  - 응답: 아래 → 위

---

## 5. 예시: 요청 시간 측정 미들웨어

```python
import time

class TimerMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        start = time.time()
        response = self.get_response(request)
        duration = time.time() - start
        print(f"⏱️ 요청 처리 시간: {duration:.4f}초")
        return response
```

---

## 6. Django 기본 제공 미들웨어

| 미들웨어 이름                                           | 설명                           |
|--------------------------------------------------------|--------------------------------|
| `SecurityMiddleware`                                   | 보안 헤더 설정                |
| `SessionMiddleware`                                    | 세션 지원                      |
| `CommonMiddleware`                                     | 다양한 기본 기능 (슬래시 처리 등) |
| `CsrfViewMiddleware`                                   | CSRF 방어                     |
| `AuthenticationMiddleware`                             | 인증 관련 사용자 정보 처리     |
| `MessageMiddleware`                                    | Django 메시지 프레임워크       |

---

## 7. 미들웨어에서 예외 처리

- 미들웨어 내부에서 예외를 처리하거나 `HttpResponse` 반환 가능
- 예외 발생 시 이후 미들웨어 및 뷰는 실행되지 않음

```python
from django.http import HttpResponse

class BlockAllMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        return HttpResponse("모든 요청이 차단되었습니다.", status=403)
```

---

## 8. 정리

- 미들웨어는 **요청과 응답 사이에 위치한 필터 또는 후크**
- Django 설정에 등록된 순서대로 실행됨
- 기본 미들웨어 외에 **사용자 정의 미들웨어**도 쉽게 작성 가능
- 인증, 보안, 로깅 등 공통 처리를 미들웨어로 구성하면 유지보수 효율 증가