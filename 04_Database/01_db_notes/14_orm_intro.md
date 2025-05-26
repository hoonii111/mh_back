# ORM Introduction

## 1. ORM이란?

- **Object-Relational Mapping**의 약자로, 객체 지향 프로그래밍 언어에서 관계형 데이터베이스를 쉽게 다룰 수 있도록 매핑해주는 기술입니다.
- 데이터베이스 테이블을 클래스, 레코드를 객체로 변환하여 SQL 없이도 데이터 조작 가능.
- 개발 생산성 향상, 코드 가독성 증가, 유지보수 용이.

## 2. ORM의 장점

- SQL 문법을 몰라도 데이터베이스 조작 가능
- 데이터베이스 독립성 확보 (DBMS 변경 시 코드 수정 최소화)
- 보안상 SQL Injection 위험 감소
- 객체 지향 패러다임에 맞춰 개발 가능

## 3. 대표적인 Python ORM 도구

| ORM 도구         | 특징                          |
|------------------|-------------------------------|
| SQLAlchemy       | 강력한 SQL 표현력과 유연성 보유 |
| Django ORM       | Django 프레임워크 내장 ORM, 사용 간편 |

---

## 4. SQLAlchemy 기초

- Core (SQL 표현)와 ORM (객체 매핑) 두 가지 방식 제공
- 주요 구성 요소:
  - **Engine**: DB 연결 관리
  - **Session**: 객체와 DB 간 상태 관리
  - **Declarative Base**: 클래스와 테이블 매핑 선언

### 예시: SQLAlchemy ORM 기본 모델 정의

```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

engine = create_engine('sqlite:///example.db', echo=True)
Base = declarative_base()

class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String)
    age = Column(Integer)

Session = sessionmaker(bind=engine)
session = Session()

# 데이터 삽입 예시
new_user = User(name='Alice', age=30)
session.add(new_user)
session.commit()