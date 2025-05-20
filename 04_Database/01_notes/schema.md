# MySQL 데이터베이스 Schema 구성

---

## 1. 스키마(Schema)란?

> **스키마(Schema)**  
데이터베이스 내 객체(테이블, 뷰, 인덱스 등)의 구조를 정의하는 논리적 단위  
MySQL에서는 **스키마 = 데이터베이스**로 취급되며 거의 동일한 의미로 사용됨.  

---

## 2. 스키마의 구성 요소

| 구성 요소         | 설명 |
|------------------|------|
| **Table (테이블)**       | 데이터를 저장하는 기본 단위 (행과 열) |
| **View (뷰)**           | 저장된 SQL 쿼리 결과를 테이블처럼 보여주는 객체 |
| **Index (인덱스)**       | 검색 속도 향상을 위한 자료구조 |
| **Trigger (트리거)**     | 특정 이벤트(INSERT, UPDATE, DELETE) 발생 시 자동 실행되는 코드 |
| **Stored Procedure / Function** | 반복되는 로직을 저장하여 호출 가능한 SQL 블록 |
| **User & Privileges (사용자 및 권한)** | 접근 제어 및 권한 설정 |

---

## 3. 스키마 생성 및 구성 예시

### 3.1 스키마 생성

```sql
CREATE DATABASE my_company;
```

---

### 3.2 테이블 생성

```sql
USE my_company;

CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50),
    hire_date DATE
);
```

---

### 3.3 뷰(View) 생성

```sql
CREATE VIEW recent_hires AS
SELECT name, hire_date
FROM employees
WHERE hire_date >= '2024-01-01';
```

---

### 3.4 인덱스(Index) 생성

```sql
CREATE INDEX idx_department ON employees(department);
```

---

### 3.5 트리거(Trigger) 생성

```sql
DELIMITER //

CREATE TRIGGER set_hire_date
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    SET NEW.hire_date = CURDATE();
END //

DELIMITER ;
```

---

## 4. 스키마 정보 확인 명령어

```sql
-- 데이터베이스 목록 보기
SHOW DATABASES;

-- 테이블 목록 보기
USE my_company;
SHOW TABLES;

-- 테이블 구조 확인
DESCRIBE employees;
```

---

## 5. 기타 참고 사항

- MySQL에서는 `스키마(Schema)`와 `데이터베이스(Database)`는 사실상 동일한 의미로 사용됨
- 다른 DBMS (Oracle, PostgreSQL 등)에서는 스키마와 데이터베이스가 구분될 수 있음

---

## ✅ 요약

- 스키마는 데이터베이스 구조의 설계도
- 구성 요소로는 테이블, 뷰, 인덱스, 트리거, 프로시저 등이 있음
- MySQL에서는 `CREATE DATABASE`로 스키마 생성, 각 요소는 `CREATE TABLE`, `CREATE VIEW` 등으로 정의