# MySQL Workbench 실습 가이드

## 1. 데이터베이스 생성 및 선택

MySQL에서는 여러 데이터베이스를 만들 수 있으므로, 실습용 DB를 따로 만들어 사용하면 좋음.

```sql
-- 실습용 데이터베이스 생성
CREATE DATABASE sql_practice;

-- 해당 데이터베이스 선택
USE sql_practice;
```

---

## 2. 테이블 생성 (DDL)

**DDL(Data Definition Language)**  
명령어는 데이터 구조를 정의

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(100)
);
```

📌 이 명령은 `users`라는 테이블을 생성하고, 세 개의 컬럼을 정의  
- `name`: 이름
- `email`: 이메일 주소

---

## 3. 데이터 삽입 (DML)

**DML(Data Manipulation Language)**  
데이터를 삽입/조회/수정/삭제하는 데 사용됨.

```sql
INSERT INTO users (id, name, email)
VALUES
  (1, 'Alice', 'alice@example.com'),
  (2, 'Bob', 'bob@example.com');
```

📌 `users` 테이블에 두 개의 사용자 데이터를 삽입  

---

## 4. 데이터 조회

```sql
SELECT * FROM users;
```

📌 현재 테이블에 저장된 모든 행을 출력합니다. `*`는 모든 컬럼을 뜻  

---

## 5. 데이터 수정

```sql
UPDATE users
SET email = 'alice@newmail.com'
WHERE id = 1;
```

📌 `id`가 1인 사용자의 이메일 주소를 업데이트  
`WHERE` 절을 빼면 모든 데이터가 수정되니 주의  

---

## 6. 데이터 삭제

```sql
DELETE FROM users
WHERE id = 2;
```

📌 `id`가 2인 사용자 데이터를 삭제  
삭제 전 조회(`SELECT`)로 대상이 맞는지 확인하는 습관이 중요  

---

## 7. 권한 제어 (DCL)

**DCL(Data Control Language)**  
명령어는 사용자에게 권한을 주거나 회수  
(관리자 계정으로 로그인해야 실행 가능)

```sql
-- 사용자 생성
CREATE USER 'test_user'@'localhost' IDENTIFIED BY 'password123';

-- SELECT 권한 부여
GRANT SELECT ON sql_practice.users TO 'test_user'@'localhost';

-- SELECT 권한 회수
REVOKE SELECT ON sql_practice.users FROM 'test_user'@'localhost';
```

📌  
- `'test_user'@'localhost'`: 로컬에서 접속하는 사용자  
- `'password123'`: 비밀번호  
- 권한 부여 후 `test_user`로 로그인하면 `users` 테이블에서 조회만 가능

---

## 8. 트랜잭션 제어 (TCL, 선택 실습)

트랜잭션은 여러 쿼리를 묶어서 실행할 때 사용하며, 중간에 실패 시 전체 취소할 수 있음.

```sql
START TRANSACTION;

INSERT INTO users (id, name, email)
VALUES (3, 'Charlie', 'charlie@example.com');

-- 테스트 후 변경 사항 취소 가능
ROLLBACK;

-- 또는 저장
COMMIT;
```

📌  
- `START TRANSACTION`: 트랜잭션 시작  
- `ROLLBACK`: 이전 상태로 되돌림  
- `COMMIT`: 변경사항 확정  

- 트랜잭션으로 시작했을 시 반드시 commit 필요.  
commit 안 할 시 임시 저장만 되는거라 workbench 종료 등 세션을 종료하면 자동 ROLLBACK됨.

---

🎉 `SELECT * FROM users;` : 최종 데이터 확인