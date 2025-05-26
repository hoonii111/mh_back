<!-- 앱 생성, 구성 요소 -->

# Django 앱 구조

Django는 프로젝트 단위로 관리되며, 각 기능은 독립적인 **앱(App)**으로 나누어 개발됩니다. 하나의 프로젝트에 여러 개의 앱이 존재할 수 있고, 앱은 기능별로 모듈화된 단위입니다.

---

## 1. ✅ 앱 생성하기

```bash
python manage.py startapp myapp
```

## 2. 주요 구성 요소 설명
 - 앱 생성 명령어 입력시 myapp 폴더 생성 및 다음과 같은 기본 구조 형성
```bash
myapp/
├── admin.py           # 관리자 사이트 설정 -> Django 관리자 페이지에 모델 등록
├── apps.py            # 앱 설정 클래스 -> 앱 이름 등 정의
├── migrations/        # 마이그레이션 파일 저장 폴더 -> 모델 변경 사항을 반영하는 마이그레이션 파일 저장
├── models.py          # 데이터베이스 모델 정의 -> 데이터베이스 테이블 구조를 정의하는 ORM 클래스 작성
├── tests.py           # 테스트 코드 작성 -> 유닛 테스트 작성용
├── views.py           # 클라이언트 요청 처리 로직 -> 클라이언트 요청을 받아 처리 후 응답 반환
├── urls.py            # (직접 생성 필요) URL 패턴 정의 -> 앱별 URL 경로 지정 (view와 연결)
```

## 3. 앱 등록
 - 앱 생성 후 settings.py의 INSTALLED_APPS에 등록해야 Django가 인식
```python
# settings.py
INSTALLED_APPS = [
    ...
    'myapp',
]
```

## 4. 앱별 urls.py 구성 예시

```python
# myapp/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```
 - 프로젝트의 urls.py에서 include()로 등록
```python
# project/urls.py
from django.urls import path, include

urlpatterns = [
    path('', include('myapp.urls')),
]
```