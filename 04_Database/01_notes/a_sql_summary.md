# Database란?
 - 데이터를 한 곳에 모아두는 정보 저장소
 - 신속하게 보고싶은 정보를 꺼낼 수 있음
 - 분석을 통해 새로운 정보를 얻을 수 있음

# SQL Summary

## 1. SQL 개념 소개
 **SQL이란?**
  - SQL(Structured Query Language)
  관계형 데이터베이스 관리 시스템에서 데이터를 관리하고 조작하기 위해 사용되는 표준 프로그래밍 언어
  - 데이터 검색, 삽입, 수정, 삭제, 스키마 생성 및 관리, 권한 제어 등 다양한 작업 수행 가능

 **주요 특징**
  - 표준화된 언어 : ANSI와 ISO에서 표준으로 제정되어 다양한 데이터베이스에서 사용 가능
  - 데이터 조작 및 정의 지원 : 데이터를 조회하고 조작하는 DML과 데이터 구조를 정의하는 DDL 명령어를 포함
  - 비절차적 언어 : 사용자가 무엇을 할지 명령하면, 데이터베이스가 어떻게 처리할지는 내부적으로 결정
  - 관계형 데이터베이스에 최적화 : 테이블 형태로 데이터를 저장, 관계 설정, 복잡한 질의 가능

 **사용 목적**
  - 데이터베이스에서 필요한 정보를 쉽게 조회
  - 데이터 추가, 수정, 삭제 등 데이터 관리
  - 데이터베이스 구조 생성 및 변경
  - 사용자 권한 및 보안 관리
  - 데이터 무결성 및 일관성 유지

## 2. SQL 명령어 종류
| 분류 | 이름 (영문) | 주요 기능 |
|------|--------------|------------|
| DDL | Data Definition Language | 데이터 구조 정의 (테이블, 뷰 등) |
| DML | Data Manipulation Language | 데이터 조회 및 조작 (조회, 삽입, 수정, 삭제) |
| DCL | Data Control Language | 사용자 권한 제어 |
| TCL | Transaction Control Language | 트랜잭션 제어 |

## 3. 각 명령어별 설명 및 예시
 ### 3.1 DDL(데이터 정의어)
  - CREATE  
  테이블, 뷰, 인덱스 등 데이터베이스 객체로 새로 생성할 때 사용
   ```sql
   CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50),
    email VARCHAR(100)
   );
   ```
  - ALTER  
  기존 테이블에 컬럼을 추가 or 수정, 삭제하는 등 구조를 변경할 때 사용
   ```sql
   ALTER TABLE users ADD COLUMM created_at TIMESTAP;
   ```
  - DROP  
  테이블이나 뷰 등의 객체를 데이터베이스에서 완전히 삭제할 때 사용
   ```sql
   DROP TABLE users;
   ```

 ### 3.2 DML(데이터 조작어)
  - SELECT  
  데이터베이스에서 데이터를 조회할 때 사용
   ```sql
   SELECT * FROM users;
   ```
  - INSERT  
  테이블에 새로운 데이터를 추가할 때 사용
   ```sql
   INSERT INTO user (id, username, email) VALUES (1, 'hoon', 'eoeo4652@naver.com')
   ```
  - UPDATE  
  기존 데이터를 수정할 때 사용
   ```sql
   UPDATE users SET email = 'eoeo4652@naver.com' WHERE id = 1;
   ```
  - DELETE  
  특정 데이터를 삭제 할 때 사용
   ```sql
   DELETE FROM users WHERE id = 1;
   ```
 ### 3.3 DCL(데이터 제어어)
  - GRANT  
  특정 사용자에게 테이블 또는 데이터베이스에 대한 접근 권한 부여
   ```sql
   GRANT SELECT, INSERT ON users TO user_name;
   ```
  - REVOKE  
  이전에 부여한 권한을 회수할 때 사용
   ```sql
   REVOKE SELECT ON users FROM user_name;
   ```
 ### 3.4 TCL(트랜잭션 제어어)
  - COMMIT  
  트랜잭션의 변경사항을 데이터베이스에 영구적으로 반영
   ```sql
   COMMIT;
   ```
  - ROLLBACK  
  트랜잭션 내에서 발생한 모든 변경을 이전 상태로 되돌림
   ```sql
   ROLLBACK;
   ```

 ## 4. 비교 요약

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