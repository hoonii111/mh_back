# Trigger

## 1. Trigger란?

- 데이터베이스 내에서 특정 이벤트(INSERT, UPDATE, DELETE)가 발생했을 때 자동으로 실행되는 저장 프로시저 같은 기능입니다.
- 데이터 무결성 유지, 자동 로그 기록, 복잡한 비즈니스 로직 구현에 사용됩니다.

## 2. Trigger의 주요 특징

- 이벤트 기반 자동 실행
- 명시적 호출 없이 동작
- BEFORE, AFTER 시점에 실행 가능
- 하나의 테이블에 여러 개의 트리거 설정 가능

## 3. Trigger 종류

| 종류       | 실행 시점             | 설명                           |
|------------|----------------------|--------------------------------|
| BEFORE INSERT  | 레코드 삽입 전에 실행 | 삽입 데이터 유효성 검사 등       |
| AFTER INSERT   | 레코드 삽입 후에 실행 | 삽입 후 로그 기록 등             |
| BEFORE UPDATE  | 레코드 수정 전에 실행 | 변경 데이터 검증                 |
| AFTER UPDATE   | 레코드 수정 후에 실행 | 변경 후 이력 관리                |
| BEFORE DELETE  | 레코드 삭제 전에 실행 | 삭제 전 참조 무결성 검사         |
| AFTER DELETE   | 레코드 삭제 후에 실행 | 삭제 후 연관 데이터 정리         |

## 4. Trigger 작성 예시 (MySQL)

```sql
DELIMITER //

CREATE TRIGGER before_user_insert
BEFORE INSERT ON users
FOR EACH ROW
BEGIN
    IF NEW.age < 0 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = '나이는 0 이상이어야 합니다.';
    END IF;
END;
//

DELIMITER ;