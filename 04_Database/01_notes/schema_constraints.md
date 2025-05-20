# MySQL Table Schema 구성 요소 정리

---

## ✅ 테이블 컬럼 제약 조건(Constraints)

테이블 설계 시 자주 사용되는 주요 제약 조건(Constraints)의 개념과 예제를 정리합니다.

---

### 1. `PRIMARY KEY`

- **역할**  
  - 테이블의 각 행을 고유하게 식별할 수 있는 칼럼  
- **특징**
  - 중복 불가(유일성 보장)
  - 검색 속도 향상
  - `NOT NULL`이 자동으로 적용됨

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50)
);
```

---

### 2. `AUTO_INCREMENT`

- **역할**  
  - 숫자형 칼럼의 값을 자동으로 1씩 증가시킴
- **사용 조건**  
  - 일반적으로 `PRIMARY KEY` 또는 `UNIQUE` 칼럼에 사용

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50)
);
```

---

### 3. `NOT NULL`

- **역할**  
  - 해당 칼럼은 `NULL` 값을 가질 수 없도록 제한
  - 해당 칼럼에는 항상 값이 존재해야 함.

```sql
	CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL
);
```

---

### 4. `UNIQUE`

- **역할**  
  - 해당 칼럼의 값은 중복된 값을 허용하지 않음.

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE
);
```

> 🔸 하나의 테이블에 여러 `UNIQUE` 컬럼 지정 가능  
> 🔸 `NULL`은 여러 번 입력 가능 (단, DBMS에 따라 다를 수 있음)

---

### 5. `DEFAULT`

- **역할**  
  - 값이 입력되지 않았을 경우 기본값을 자동으로 설정

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    is_business VARCHAR(10) DEFAULT False
);
```

---

### 6. `CHECK`

- **역할**  
  - 해당 칼럼에 저장될 수 있는 값의 범위나 조건을 지정


```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    is_business VARCHAR(10) DEFAULT False,
    age INT CHECK (age >= 18)
);
```

---

## 🧠 참고: 여러 제약 조건 함께 사용 예시

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT CHECK (age >= 18),
    status VARCHAR(10) DEFAULT 'active'
);
```

---

## 🔍 관련 명령어 요약

| 제약 조건     | 설명                             |
|--------------|----------------------------------|
| `PRIMARY KEY`| 고유 식별자, 중복/NULL 불가      |
| `AUTO_INCREMENT` | 자동 증가 숫자 (주로 PK와 함께) |
| `NOT NULL`   | NULL 입력 방지                    |
| `UNIQUE`     | 중복 데이터 입력 방지             |
| `DEFAULT`    | 기본값 설정                       |
| `CHECK`      | 값의 조건 지정 (MySQL 8.0 이상)   |

---

## ✅ 요약

- 제약 조건은 **데이터 무결성**을 지키기 위한 테이블 설계의 핵심
- 여러 제약 조건을 **복합적으로 설정**해 안정적인 DB 구조를 만들 수 있음