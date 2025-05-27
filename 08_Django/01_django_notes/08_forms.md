<!-- 폼 생성, 유효성 검사, 처리 -->

# Django Forms

## 1. 폼이란?

 - 사용자 입력을 처리하는 HTML 구조
 - Django에서는 `Form` 클래스로 폼 생성, 유효성 검사, 전송 처리 가능함
 - HTML 폼 생성부터, 입력값 검증, DB 저장까지 통합적으로 다룰 수 있음

---

## 2. 폼 종류

| 폼 종류         | 설명                                      |
|----------------|-------------------------------------------|
| `forms.Form`   | 일반적인 폼. DB와 무관함                   |
| `forms.ModelForm` | 모델과 연결된 폼. DB와 자동 연동됨         |

---

## 3. Form 클래스 정의

```python
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
    message = forms.CharField(widget=forms.Textarea)
```
 - 필드 종류에 따라 다양한 유효성 검사가 자동 수행됨

---

## 4. 폼 렌더링

### 뷰에서 처리

```python
def contact_view(request):
    if request.method == 'POST':
        form = ContactForm(request.POST)
        if form.is_valid():
            # 처리 로직
            return redirect('success')
    else:
        form = ContactForm()
    return render(request, 'contact.html', {'form': form})
```

### 템플릿에서 출력

```html
<form method="post">
  {% csrf_token %}
  {{ form.as_p }}
  <button type="submit">전송</button>
</form>
```
| 출력 방식           | 설명            |
| --------------- | ------------- |
| `form.as_p`     | `<p>` 태그로 출력  |
| `form.as_table` | `<tr>` 태그로 출력 |
| `form.as_ul`    | `<li>` 태그로 출력 |

---

## 5. 유효성 검사

 - **is_calid()** 호출 시, 내부적으로 각 필드에 대해 유효성 검사 수행
 - 유효하지 않은 경우 **form.errors**에 오류 정보 저장됨

### 필드 단위 유효성 검사

```python
def clean_email(self):
    email = self.cleaned_data['email']
    if not email.endswith('@example.com'):
        raise forms.ValidationError('example.com 도메인만 허용됨')
    return email
```

### 전체 폼 유효성 검사

```python
def clean(self):
    cleaned_data = super().clean()
    password = cleaned_data.get('password')
    confirm = cleaned_data.get('confirm')
    if password and confirm and password != confirm:
        raise forms.ValidationError('비밀번호 일치하지 않음')
```

---

## 6. ModelForm

 - 모델 기반으로 폼 자동 생성
 - **save()** 호출 시 DB 저장까지 처리됨

### 정의 예시

```python
from django.forms import ModelForm
from .models import Article

class ArticleForm(ModelForm):
    class Meta:
        model = Article
        fields = ['title', 'content']
```

### 사용 예시

```python
def article_create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('article_list')
    else:
        form = ArticleForm()
    return render(request, 'articles/form.html', {'form': form})
```

---

## 7. 위젯

 - 폼 필드의 HTML 표현 방식 지정할 수 있음

```python
class CustomForm(forms.Form):
    name = forms.CharField(widget=forms.TextInput(attrs={'class': 'form-control'}))
    message = forms.CharField(widget=forms.Textarea(attrs={'rows': 5}))
```

---

## 8. 폼 클래스 커스터마이징

 - 필드 라벨, 도움말, 기본값 지정 가능

```python
class CustomForm(forms.Form):
    username = forms.CharField(
        label='사용자명',
        help_text='영문자와 숫자 조합',
        initial='guest',
        max_length=20
    )
```

---

## 9. 요약

 - Django 폼 = 사용자 입력을 구조적으로 처리하기 위한 도구
 - **Form** = 일반 폼, **ModelForm** = DB 연동
 - 유효성 검사, 위젯 설정, 출력 방식 등 다양한 기능 내장
 - 템플릿과 함께 사용하여 강력한 폼 처리 가능