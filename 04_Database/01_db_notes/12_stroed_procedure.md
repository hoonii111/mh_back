# Stored Procedures in MySQL

## 📌 1. 저장 프로시저란?

저장 프로시저(Stored Procedure) = **자주 사용하는 SQL 문장들을 모듈화하여 저장한 사용자 정의 프로그램**
일련의 SQL 명령문들을 저장해 두고, 필요할 때마다 호출해서 실행 가능

- 코드 재사용성 증가
- 복잡한 비즈니스 로직 캡슐화
- 성능 개선 (네트워크 왕복 감소)
- 보안성 향상 (권한 제어 용이)

---

## ⚙️ 2. 저장 프로시저 생성

```sql
DELIMITER //
CREATE PROCEDURE getAllUsers()
BEGIN
    SELECT * FROM users;
END //
DELIMITER ;
```

- `DELIMITER`는 MySQL이 세미콜론(`;`) 대신 명령어 종료를 인식할 구분자를 변경함.
- `CREATE PROCEDURE`로 저장 프로시저 생성
- `getAllUsers()`는 매개변수가 없는 예시

---

## 🧮 3. 매개변수(Parameter)

### 🔹 IN: 입력값을 전달할 때 사용

```sql
DELIMITER //
CREATE PROCEDURE getUserById(IN userId INT)
BEGIN
    SELECT * FROM users WHERE id = userId;
END //
DELIMITER ;
```

### 🔹 OUT: 결과를 반환할 때 사용

```sql
DELIMITER //
CREATE PROCEDURE getUserCount(OUT total INT)
BEGIN
    SELECT COUNT(*) INTO total FROM users;
END //
DELIMITER ;
```

### 🔹 INOUT: 입력도 받고 수정 후 출력도 가능

```sql
DELIMITER //
CREATE PROCEDURE doubleValue(INOUT num INT)
BEGIN
    SET num = num * 2;
END //
DELIMITER ;
```

---

## 🚀 4. 프로시저 실행 방법

```sql
CALL getAllUsers();

CALL getUserById(3);

-- OUT/INOUT 사용 시 변수 선언 후 호출
SET @count = 0;
CALL getUserCount(@count);
SELECT @count;

SET @value = 5;
CALL doubleValue(@value);
SELECT @value;
```

---

## 🛠️ 5. 저장 프로시저 관리

- 프로시저 목록 보기  
  ```sql
  SHOW PROCEDURE STATUS WHERE Db = 'your_database_name';
  ```

- 프로시저 정의 보기  
  ```sql
  SHOW CREATE PROCEDURE getAllUsers;
  ```

- 프로시저 삭제  
  ```sql
  DROP PROCEDURE IF EXISTS getAllUsers;
  ```

---

## ✅ 6. 활용 예시

- 로그인 로직 구현
- 사용자 등록, 삭제 자동 처리
- 보고서 생성 쿼리 캡슐화

---

> 🔍 **Tip**: 복잡한 로직이 자주 반복된다면 저장 프로시저로 관리하면 유지보수가 쉬워지고 퍼포먼스도 향상됩니다.