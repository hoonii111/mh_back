<!-- 모델 정의, ORM 기본, 마이그레이션 -->

# Django Models

## 1. 모델이란?

- Django의 모델(Model)은 데이터베이스와 상호작용하는 데이터 구조를 정의하는 구성 요소입니다.
- `django.db.models.Model` 클래스를 상속받아 정의하며, Django ORM(Object-Relational Mapping)을 통해 SQL 없이 데이터 조작이 가능합니다.
- 각 앱의 `models.py` 파일 내에서 정의합니다.

## 2. 모델 기본 구조

```python
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title
```
 - **CharField** : 길이 제한이 있는 문자열  
 - **TextField** : 긴 텍스트 필드  
 - **DateTimeField** : 날짜 및 시간 저장  
 - **auto_now_add=True** : 생성 시 자동 저장  
 - **auto_now=True** : 수정 시 자동 저장  
 - **__str__** : 객체를 사람이 읽기 쉽게 문자열로 표현  

## 3. 주요 필드 타입

| 필드 클래스          | 설명                       |
| --------------- | ------------------------ |
| `CharField`     | 짧은 문자열. `max_length` 필수  |
| `TextField`     | 긴 텍스트                    |
| `IntegerField`  | 정수                       |
| `FloatField`    | 부동 소수점 숫자                |
| `BooleanField`  | 참/거짓 값                   |
| `DateTimeField` | 날짜와 시간                   |
| `DateField`     | 날짜만                      |
| `TimeField`     | 시간만                      |
| `EmailField`    | 이메일 형식 유효성 검사 포함         |
| `URLField`      | URL 유효성 검사 포함            |
| `FileField`     | 파일 업로드 필드                |
| `ImageField`    | 이미지 파일 업로드 (`Pillow` 필요) |

## 4. 모델 간 관계 설정

### 1:1 관계 (OneToOneField)
```python
class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
```

### 1:N 관계 (ForeingKey)
```python
class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
```

### N:N 관계 (ManyToManyField)
```python
class Author(models.Model):
    books = models.ManyToManyField(Book)
```

## 5. Meta 클래스 (모델 옵션)
 - 모델 내부에 **class Meta**를 정의하여 추가 설정 가능  
```python
class Article(models.Model):
    title = models.CharField(max_length=200)

    class Meta:
        ordering = ['-id']  # 최신 글부터 정렬
        verbose_name = '게시글'
        verbose_name_plural = '게시글 목록'
```
 - **ordering** : 기본 정렬 순서
 - **verbose_name** : 단수 이름
 - **verbose_name_plural** : 복수 이름

## 6. Migration
 - 모델을 정의하거나 수정한 후, 이를 데이터베이스에 반영하기 위해 마이그레이션을 수행

### 마이그레이션 명령어
```bash
python manage.py makemigrations   # 마이그레이션 파일 생성
python manage.py migrate          # 실제 DB에 적용
python manage.py showmigrations   # 마이그레이션 적용 현황 확인
```

### 마이그레이션 흐름
1. 모델 정의 or 수정
2. **makemigrations** -> 변경 사항을 담은 파일 생성
3. **migrate** -> 데이터베이스에 실제 반영

## 7. ORM을 통한 객체 조작

### 객체 생성
```python
article = Article.objects.create(title='제목', content='내용')
```
### 조회
```python
Article.objects.all()
Article.objects.get(id=1)
Article.objects.filter(title__contains='파이썬')
```
### 수정
```python
article = Article.objects.get(id=1)
article.title = '수정된 제목'
article.save()
```
### 삭제
```python
article.delete()
```