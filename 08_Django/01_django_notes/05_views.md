<!-- 뷰 함수와 클래스 기반 뷰(CBV) 소개 -->

# Django Views

## 1. 뷰(View)란?

 - Django의 뷰 = 사용자의 요청(request) 처리, 적절한 응답(response) 반환
 - 비즈니스 로직을 담당, 모델과 템플릿 사이 중개자 역할 수행
 - 뷰는 함수 기반 뷰(Function-Based View, FBV)와 클래스 기반 뷰(Class-Based View, CBV) 두 가지 방식으로 작성 가능

---

## 2. 함수 기반 뷰 (Function-Based View)

 - 가장 기본적인 뷰 작성 방법
 - 요청을 매개변수로 받아 처리하고, `HttpResponse`를 반환

### 기본 예시

```python
from django.http import HttpResponse

def hello_world(request):
    return HttpResponse("Hello, world!")
```

---

## 3. 템플릿 렌더링 예시

```python
from django.shortcuts import render

def article_list(request):
    articles = Article.objects.all()
    return render(request, 'articles/list.html', {'articles': articles})
```
 - **render()** : 템플릿 파일과 데이터를 조합해 HTML을 반환

---

## 4. 클래스 기반 뷰

 - **CBV** = Class-Based View  
 - 뷰를 클래스 형태로 작성하여 재사용성과 확장성 높일 수 있음.  
 - Django = 다양한 기본 CBV 제공, 상속받아 사용 가능  

### 기본 CBV 예시

```python
from django.views import View
from django.http import HttpResponse

class HelloWorldView(View):
    def get(self, request):
        return HttpResponse("Hello, world!")
```

### 주요 CBV 종류

| 뷰 클래스          | 설명             |
| -------------- | -------------- |
| `View`         | 기본 뷰 클래스       |
| `TemplateView` | 템플릿 렌더링용 뷰     |
| `ListView`     | 객체 목록 조회용 뷰    |
| `DetailView`   | 단일 객체 상세 조회용 뷰 |
| `CreateView`   | 폼을 통한 객체 생성 뷰  |
| `UpdateView`   | 폼을 통한 객체 수정 뷰  |
| `DeleteView`   | 객체 삭제 뷰        |

---

## 5. CBV 템플릿 뷰 예시

```python
from django.views.generic import TemplateView

class AboutView(TemplateView):
    template_name = 'about.html'
```

---

## 6. ListView 예시

```python
from django.views.generic import ListView

class ArticleListView(ListView):
    model = Article
    template_name = 'articles/list.html'
    context_object_name = 'articles'  # 템플릿에서 사용할 이름
    paginate_by = 10  # 페이지당 표시할 객체 수
```

---

## 7. DetailView 예시

```python
from django.views.generic import DetailView

class ArticleDetailView(DetailView):
    model = Article
    template_name = 'articles/detail.html'
    context_object_name = 'article'
```

---

## 8. 뷰에서 URL 매핑 및 사용법

 - 뷰를 작성한 후, **urls.py**에 연결해줘야 접근할 수 있음.
```python
from django.urls import path
from .views import article_list, ArticleDetailView

urlpatterns = [
    path('articles/', article_list, name='article_list'),               # 함수 기반 뷰
    path('articles/<int:pk>/', ArticleDetailView.as_view(), name='article_detail'),  # 클래스 기반 뷰
]
```
 - 클래스 기반 뷰는 **as_view()** 메서드를 사용해 URL에 연결

---

## 9. 요청 메서드별 처리 (함수 기반)

```python
from django.http import HttpResponseNotAllowed

def my_view(request):
    if request.method == 'GET':
        # GET 요청 처리
        return HttpResponse("GET 요청입니다.")
    elif request.method == 'POST':
        # POST 요청 처리
        return HttpResponse("POST 요청입니다.")
    else:
        return HttpResponseNotAllowed(['GET', 'POST'])
```

---

## 10. CBV에서 요청 메서드 처리

 - **view** 클래스를 상속받아 **get()**, **post()** 등 메서드를 오버라이드함.

```python
from django.views import View
from django.http import HttpResponse

class MyView(View):
    def get(self, request):
        return HttpResponse("GET 요청 처리")
    
    def post(self, request):
        return HttpResponse("POST 요청 처리")
```
---

## 11. 요약 

 - 뷰 = 사용자의 요청을 처리하고 응답을 반환하는 핵심 컴포넌트
 - 함수 기반 뷰 -> 간단, 직관적 / 클래스 기반 뷰 -> 재사용성, 확장성
 - Django -> 다양한 제네릭 CBV 제공 => CRUD 작업을 쉽고 빠르게 구현
 - URLconf에 뷰를 연결해 외부 요청과 매핑해야함.