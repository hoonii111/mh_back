<!-- URL 라우팅, path, re_path 사용법 -->

# Django URL Routing

## 1. URL 라우팅이란?

- URLconf(URL configuration)는 사용자의 요청 URL을 적절한 뷰(view)로 연결해주는 역할을 합니다.
- Django에서는 각 앱 또는 프로젝트 단위로 `urls.py` 파일을 사용하여 URL 경로를 정의합니다.

---

## 2. 기본 구조

```python
# 프로젝트의 urls.py (예: config/urls.py)
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
]

# 앱의 urls.py (예: articles/urls.py)
from django.urls import path
from . import views

urlpatterns = [
    path('', views.article_list, name='article_list'),
    path('<int:pk>/', views.article_detail, name='article_detail'),
]
```

---

## 3. paht() 함수

```python
path(route, view, kwargs=None, name=None)
```

| 매개변수     | 설명                                     |
| -------- | -------------------------------------- |
| `route`  | URL 패턴 문자열 (예: `'articles/<int:pk>/'`) |
| `view`   | 연결할 뷰 함수 또는 클래스 (`as_view()` 필요)       |
| `kwargs` | 추가 인자 (잘 사용되진 않음)                      |
| `name`   | URL의 이름 (템플릿에서 `{% url %}`로 사용 가능)     |

---

## 4. 경로 매개변수 타입

| 경로 변환기 | 설명             |
| ------ | -------------- |
| `str`  | 문자열 (기본값)      |
| `int`  | 정수             |
| `slug` | 문자, 숫자, 하이픈 포함 |
| `uuid` | UUID 형식        |
| `path` | `/` 포함 전체 경로   |

### 예시

```python
path('articles/<int:pk>/', views.article_detail),
path('users/<slug:username>/', views.user_profile),
```

---

## 5. **name**을 활용한 URL Reverse

 - URL 패턴에 **name**을 지정하면 템플릿이나 뷰에서 이름으로 URL 참조 가능
### ex) 템플릿
```html
<a href="{% url 'article_detail' pk=article.id %}">상세보기</a>
```

### ex) 뷰
```python
from django.urls import reverse

url = reverse('article_detail', kwargs={'pk': 1})
```

---

## 6. include() 함수

 - 앱별 URLconf를 분리하여 모듈화할 수 있도록 도와줌
 - **path()** 함수 내에서 **include('앱이릅.urls')** 형식으로 사용

### 예시

```python
path('blog/', include('blog.urls'))
```

---

## 7. re_path() 함수

 - 정규 표현식을 사용한 URL 패턴 정의에 사용됨
 - 특별한 경우가 아니면 **path()** 사용 권장
```python
from django.urls import re_path

urlpatterns = [
    re_path(r'^archive/(?P<year>[0-9]{4})/$', views.archive),
]
```

---

## 8. URL 네임스페이스(namespace)
 - 동일한 이름의 URL이 여러 앱에 있을 때 충돌 방지를 위해 사용
```python
# 프로젝트 urls.py
path('shop/', include('shop.urls', namespace='shop'))

# shop/urls.py
app_name = 'shop'

urlpatterns = [
    path('item/<int:pk>/', views.item_detail, name='item_detail'),
]
```

### 사용 예시
```html
<a href="{% url 'shop:item_detail' pk=3 %}">상품 상세</a>
```

---

## 9. 요약

 - Django는 **urls.py**를 통해 요청 URL을 각 뷰로 연결
 - **path()**를 기본 사용, 필요 시 **re_path()** 사용
 - 앱별로 URLconf를 분리해 **include()**로 연결 => 유지보수 쉬워짐
 - URL에 **name**을 부여 => 템플릿 및 뷰에서 역참조가 가능 (유연한 URL 관리)