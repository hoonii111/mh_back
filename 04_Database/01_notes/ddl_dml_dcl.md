# DDL, DML, DCL 개념 정리



## 1. SQL 명령어 분류

SQL은 기능에 따라 다음과 같이 분류:  

| 분류 | 이름 (영문) | 주요 기능 |
|------|--------------|------------|
| DDL | Data Definition Language | 데이터 구조 정의 (테이블, 뷰 등) |
| DML | Data Manipulation Language | 데이터 조회 및 조작 (조회, 삽입, 수정, 삭제) |
| DCL | Data Control Language | 사용자 권한 제어 |

---

## 2. DDL (데이터 정의어)  
 데이터베이스의 구조를 정의하는데 사용.


### 주요 명령어 및 예시

- **CREATE**  
  새로운 테이블이나 데이터베이스 생성
  ```sql
  CREATE TABLE users (
      id INT PRIMARY KEY,
      name VARCHAR(50),
      email VARCHAR(100)
  );
  ```
  

- **ALTER**  
  기존 테이블이나 데이터베이스 구조 수정
  ```sql
  ALTER TABLE users ADD COLUMN created_at TIMESTAMP;
  ```

- **DROP**  
  테이블이나 데이터베이스 삭제
  ```sql
  DROP TABLE users;
  ```

---

## 3. DML (데이터 조작어)   
 데이터베이스 내의 데이터를 처리하는 데 사용되는 언어.

### 주요 명령어 및 예시

- **SELECT**  
  데이터 조회
  ```sql
  SELECT * FROM users;
  ```

- **INSERT**  
  새로운 데이터 삽입
  ```sql
  INSERT INTO users (id, name, email)
  VALUES (1, 'Alice', 'alice@example.com');
  ```

- **UPDATE**  
  기존 데이터 수정
  ```sql
  UPDATE users SET email = 'alice@newmail.com' WHERE id = 1;
  ```

- **DELETE**  
  특정 데이터 삭제
  ```sql
  DELETE FROM users WHERE id = 1;
  ```

---

## 4. DCL (데이터 제어어)  
 데이터베이스 사용자의 권한을 관리하고 데이터 접근을 제어하는데 사용되는 언어.

### 주요 명령어 및 예시

- **GRANT**  
  사용자에게 특정 데이터에 대한 접근 권한 부여
  ```sql
  GRANT SELECT, INSERT ON users TO user_name;
  ```

- **REVOKE**  
  사용자의 특정 데이터 접근 권한 제거
  ```sql
  REVOKE INSERT ON users FROM user_name;
  ```

---

## 5. TCL  
 데이터베이스 내의 트랜잭션을 관리하는데 사용되는 언어.

### 주요 명령어 및 예시

- **COMMIT**
  트랜잭션을 완료하고, 데이터베이스 변경사항을 영구적으로 저장

- **ROLLBACK**
  트랜잭션을 취소하고, 마지막 COMMIT 이후의 모든 변경사항을 되돌림

- **SAVEPOINT**
  트랜잭션 내 특정 지점을 마킹하여 필요시 그 지점으로 되돌릴 수 있음.

---

## 6. 비교 요약

| 분류 | 설명 | 예시 명령어 |
|------|------|--------------|
| DDL | 데이터 구조 정의 | `CREATE`, `ALTER`, `DROP` |
| DML | 데이터 조작 | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| DCL | 권한 관리 | `GRANT`, `REVOKE` |
| TCL | 트랜잭션 관리 | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

---

## 🔍 참고 팁

- DDL, DCL 명령어는 실행 시 자동으로 COMMIT 처리됨 (되돌리기 불가)
- DML은 트랜잭션 처리를 통해 COMMIT / ROLLBACK 가능
- 실무에서는 DML이 가장 자주 사용됨