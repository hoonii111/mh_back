# Security Practices in SQL

## 1. SQL Injection이란?

- 공격자가 악의적인 SQL 코드를 삽입해 데이터베이스를 조작하거나 민감 정보를 탈취하는 공격 기법.
- 입력값을 제대로 검증하지 않아 발생.
- 심각한 보안 위협으로 간주됨.

## 2. SQL Injection 방지 방법

### 2.1. Prepared Statements (파라미터화된 쿼리)

- SQL 문과 데이터 값을 분리하여 쿼리 실행.
- 쿼리 내 데이터가 코드로 해석되지 않음.
- 대부분의 DB 라이브러리 및 ORM에서 지원.

```python
# Python 예시 (MySQL Connector)
cursor.execute("SELECT * FROM users WHERE username = %s", (username,))