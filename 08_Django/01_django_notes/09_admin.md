<!-- Admin 사이트 설정 및 커스터마이징 -->

# 09. Django Admin

## 관리자 사이트란?

- Django에서 기본 제공하는 백오피스 관리 도구
- 모델 기반 데이터 생성, 수정, 삭제 가능
- 별도 개발 없이 자동으로 인터페이스 생성됨

---

## 1. 관리자 사이트 활성화

- `django.contrib.admin` 앱이 `INSTALLED_APPS`에 포함돼 있어야 함
- 기본 설정 시 `/admin/` 경로로 접속 가능

```python
# urls.py
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

---

## 2. superuser 생성

 - 관리자 접속을 위해 **superuser** 계정 필요

```bash
python manage.py createsuperuser
```
 - 이메일, 사용자명, 비밀번호 입력하면 생성됨

---

## 3. 모델 등록

 - 모델이 관리자 사이트에 표시되려면 **admin.py**에서 등록해야함

```python
from django.contrib import admin
from .models import Article

admin.site.register(Article)
```

---

## 5. ModelAdmin 클래스 커스터마이징

 - **ModelAdmin** 클래스 상속해서 다양한 옵션 설정 가능

```python
class ArticleAdmin(admin.ModelAdmin):
    list_display = ['id', 'title', 'author', 'created_at']
    search_fields = ['title', 'content']
    list_filter = ['created_at']
    ordering = ['-created_at']

admin.site.register(Article, ArticleAdmin)
```

| 옵션                | 설명                 |
| ----------------- | ------------------ |
| `list_display`    | 목록에서 보여줄 필드 목록 지정  |
| `search_fields`   | 검색창에서 검색할 필드 지정    |
| `list_filter`     | 우측 필터바에서 필터할 필드 지정 |
| `ordering`        | 기본 정렬 순서 지정        |
| `readonly_fields` | 읽기 전용 필드 지정        |

---

## 6. 필드 구성 제어

 - 입력 폼에서 보여줄 필드 순서 및 그룹화 가능

```python
class ArticleAdmin(admin.ModelAdmin):
    fields = ['title', 'content', 'author']
    fieldsets = [
        ('기본 정보', {'fields': ['title', 'author']}),
        ('내용', {'fields': ['content']}),
    ]
```

---

### 7. 인라인 모델 관리

 - 관계 모델을 같은 화면에서 함께 편집 가능

```python
class CommentInline(admin.TabularInline):
    model = Comment
    extra = 1

class ArticleAdmin(admin.ModelAdmin):
    inlines = [CommentInline]
```
| 클래스             | 설명       |
| --------------- | -------- |
| `TabularInline` | 표 형식 인라인 |
| `StackedInline` | 폼 형식 인라인 |

---

## 8. 커스터마이징 예제 : 사용자 정의 관리자

```python
class CustomUserAdmin(admin.ModelAdmin):
    list_display = ['username', 'email', 'is_staff']
    list_editable = ['is_staff']
    search_fields = ['username', 'email']

admin.site.register(CustomUser, CustomUserAdmin)
```

## 9. Admin 사이트 커스터마이징

```python
# admin.py
admin.site.site_header = "내부 관리자 페이지"
admin.site.site_title = "Admin Portal"
admin.site.index_title = "관리 기능 목록"
```

---

## 10. 관리자 페이지에 CSS/JS 추가

 - **Media** 클래스 이용하여 static 파일 삽입 가능


```python
class ArticleAdmin(admin.ModelAdmin):
    class Media:
        css = {'all': ('css/admin.css',)}
        js = ('js/admin.js',)
```

---

## 11. 요약

 - Django Admin = 개발 없이도 빠르게 백오피스 기능 구현 가능
 - **ModelAdmin** 커스터마이징으로 목록, 필터, 검색 등 정교하게 설정 가능
 - 인라인 관리, 레이아웃 제어 등 다양한 확장성 제고
 - **site_header**, **site_title** 등을 통해 관리자 사이트 브랜딩 가능