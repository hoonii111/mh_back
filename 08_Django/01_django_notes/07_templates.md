<!-- 템플릿 시스템과 템플릿 언어 기초 -->

# Django Templates

## 1. 템플릿 시스템이란?

- Django에서 HTML 출력 담당하는 계층
- 동적 데이터를 HTML과 결합해 사용자에게 보여줌
- 자체 템플릿 언어(DTL, Django Template Language) 사용함

---

## 2. 템플릿 파일 위치

- 앱 디렉토리 내 `templates/` 하위에 HTML 파일 위치시킴
- 또는 프로젝트 전역에 `templates/` 디렉토리 만들어 `settings.py`에 등록 가능

```bash
myapp/
├── templates/
│   └── myapp/
│       └── example.html
```
```python
# settings.py
TEMPLATES = [
    {
        ...
        'DIRS': [BASE_DIR / 'templates'],
        ...
    },
]
```

---

## 3. 템플릿 렌더링

 - **render()** 함수로 템플릿과 context 데이터 결함 후 HTML 응답 반환

```python
from django.shortcuts import render

def article_list(request):
    articles = Article.objects.all()
    return render(request, 'articles/list.html', {'articles': articles})
```

---

## 4. 템플릿 변수 출력

 - **{{ 변수 }}** 형태로 출력 가능
```html
<p>{{ article.title }}</p>
<p>{{ user.usename }}</p>
```

---

## 5. 필터

 - 출력값 가공 시 사용

```html
<p>{{ article.created_at|date:"Y-m-d" }}</p>
<p>{{ content|truncatewords:30 }}</p>
```

| 필터              | 설명            |
| --------------- | ------------- |
| `date`          | 날짜 포맷 지정      |
| `length`        | 문자열 길이 반환     |
| `default`       | 값 없을 때 기본값 출력 |
| `lower`/`upper` | 소문자/대문자 변환    |

---

## 6. 템플릿 태그

 - 조건문, 반복문 등 제어 구조 제공

### 조건문

```html
{% if user.is_authenticated %}
  <p>환영함, {{ user.username }}</p>
{% else %}
  <p>로그인 필요함</p>
{% endif %}
```

### 반복문

```html
<ul>
  {% for article in articles %}
    <li>{{ article.title }}</li>
  {% endfor %}
</ul>
```

### 기타 태그

```html
{% comment "설명" %} 숨김처리 {% endcomment %}
{% include 'partials/footer.html' %}
```

---

## 7. 상속 (Template Inheritance)

 - 템플릿 재사용 위한 구조 제공

### base.html

```html
<!DOCTYPE html>
<html>
<head>
  <title>{% block title %}기본 제목{% endblock %}</title>
</head>
<body>
  <div class="content">
    {% block content %}{% endblock %}
  </div>
</body>
</html>
```

### child.html

```html
{% extends 'base.html' %}

{% block title %}게시글 목록{% endblock %}

{% block content %}
  <h1>게시글 목록</h1>
  {% for article in articles %}
    <p>{{ article.title }}</p>
  {% endfor %}
{% endblock %}
```

---

## 8. 정적 파일 로딩

 - CSS, JS 등 정적 자원은 **{% static %}** 태그 사용함

```html
{% load static %}

<link rel="stylesheet" href="{% static 'css/styles.css' %}">
<img src="{% static 'images/logo.png' %}" alt="로고">
```

---

## 9. URL 역참조

 - 템플릿에서 URL name 사용해 동적 링크 생성 가능

```html
<a href="{% url 'article_detail' pk=article.id %}">자세히 보기</a>
```

---

## 10. 템플릿 디버깅 팁

 - **{{ 변수|default:"없음" }}** 필터로 안전하게 출력 가능
 - **{% debug %}** 사용 시 context 내용 전체 출력됨 (개발시 유용)

## 11. 요약

 - 템플릿은 HTML과 데이터를 결합해 사용자에게 출력하는 표현 계층임
 - 변수( **{{}}** ), 태그( **{%}** ), 필터( **|** ) 조합해 동적 페이지 생성
 - 상속 구조를 통해 중복을 줄이고 유지보수 편리
 - 정적 자원, URL 등 다른 기능과 쉽게 연동됨.