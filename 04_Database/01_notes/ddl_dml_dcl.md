# DDL, DML, DCL 개념 정리



## 1. SQL 명령어 분류

SQL은 기능에 따라 다음과 같이 분류됩니다:

| 분류 | 이름 (영문) | 주요 기능 |
|------|--------------|------------|
| DDL | Data Definition Language | 데이터 구조 정의 (테이블, 뷰 등) |
| DML | Data Manipulation Language | 데이터 조회 및 조작 (조회, 삽입, 수정, 삭제) |
| DCL | Data Control Language | 사용자 권한 제어 |

---

## 2. DDL (데이터 정의어)

**정의**: 데이터베이스의 **스키마(구조)를 정의하거나 변경**할 때 사용하는 명령어입니다.  
→ 테이블 생성, 수정, 삭제 등

### 주요 명령어 및 예시

- **CREATE**  
  테이블 또는 기타 객체를 생성합니다.
  ```sql
  CREATE TABLE users (
      id INT PRIMARY KEY,
      name VARCHAR(50),
      email VARCHAR(100)
  );
  ```
  

- **ALTER**  
  기존 테이블의 구조를 수정합니다.
  ```sql
  ALTER TABLE users ADD COLUMN created_at TIMESTAMP;
  ```

- **DROP**  
  테이블 또는 객체를 삭제합니다.
  ```sql
  DROP TABLE users;
  ```

---

## 3. DML (데이터 조작어)

**정의**: 데이터베이스에 **저장된 데이터를 조회, 삽입, 수정, 삭제**할 때 사용하는 명령어입니다.  
→ 실무에서 가장 자주 사용됨

### 주요 명령어 및 예시

- **SELECT**  
  데이터를 조회합니다.
  ```sql
  SELECT * FROM users;
  ```

- **INSERT**  
  새 데이터를 삽입합니다.
  ```sql
  INSERT INTO users (id, name, email)
  VALUES (1, 'Alice', 'alice@example.com');
  ```

- **UPDATE**  
  기존 데이터를 수정합니다.
  ```sql
  UPDATE users SET email = 'alice@newmail.com' WHERE id = 1;
  ```

- **DELETE**  
  특정 데이터를 삭제합니다.
  ```sql
  DELETE FROM users WHERE id = 1;
  ```

---

## 4. DCL (데이터 제어어)

**정의**: 사용자에게 **데이터에 대한 권한을 부여하거나 회수**할 때 사용하는 명령어입니다.

### 주요 명령어 및 예시

- **GRANT**  
  사용자에게 특정 권한을 부여합니다.
  ```sql
  GRANT SELECT, INSERT ON users TO user_name;
  ```

- **REVOKE**  
  권한을 회수합니다.
  ```sql
  REVOKE INSERT ON users FROM user_name;
  ```

---

## 5. 비교 요약

| 분류 | 설명 | 예시 명령어 |
|------|------|--------------|
| DDL | 데이터 구조 정의 | `CREATE`, `ALTER`, `DROP` |
| DML | 데이터 조작 | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| DCL | 권한 관리 | `GRANT`, `REVOKE` |

---

## 🔍 참고 팁

- DDL, DCL 명령어는 실행 시 자동으로 COMMIT 처리됨 (되돌리기 불가)
- DML은 트랜잭션 처리를 통해 COMMIT / ROLLBACK 가능
- 실무에서는 DML이 가장 자주 사용됨