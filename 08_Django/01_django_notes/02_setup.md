<!-- Django 프로젝트 생성, 설정, 기본 구조 -->
# Project Setup

## 1. Django 프로젝트 생성

#### 1. 가상환경 생성 및 활성화 (권장)
```bash
python -m venv venv
source venv/bin/activate  # macOS/Linux
venv\Scripts\activate     # Windows
```
### 2. Django 설치
```bash
pip install django
```
### 3. 프로젝트 생성
```bash
django-admin startproject projectname
```
### 4. 프로젝트 디렉토리 구조
```bash
projectname/
├── manage.py           # 프로젝트 관리 명령어 실행 파일
└── projectname/
    ├── __init__.py
    ├── settings.py     # 프로젝트 설정 파일
    ├── urls.py         # URL 라우팅 정의
    └── wsgi.py         # WSGI 웹 서버 게이트웨이 인터페이스
```
### 5. 개발 서버 실행
```bash
python manage.py runserver
# 기본적으로 http://127.0.0.1:8000/ 에서 개발 서버 구동
```

---

## 2. 주요 설정

settings.py

 - INSTALLED_APPS: 프로젝트에 포함된 앱 리스트

 - DATABASES: 데이터베이스 설정 (기본 SQLite)

 - ALLOWED_HOSTS: 서비스 도메인 또는 IP 지정

 - SECRET_KEY: 보안에 중요한 임의 문자열

---

## 3. 주요 관리 명령어

```bash
python manage.py migrate # 데이터베이스 마이그레이션 적용
python manage.py makemigrations # 모델 변경사항을 마이그레이션 파일로 생성
python manage.py createsuperuser # 관리자 계정 생성
```

---

## 4. PyCharm 기본 셋팅

### 1. pyenv 세팅
```bash
pyenv virtualenv 3.12.9 <name>
cd <해당 경로>
pyenv local <name>
```
### 2. poetry 셋팅
```bash
poetry init
poetry config virtualenvs.create false --local
poeyry add django
```
### 3. 테스트
```bash
pip list # 파일 중 Django 5.2가 보여야 성공
```
### 4. 시작
```bash
django-admin startproject config .
```