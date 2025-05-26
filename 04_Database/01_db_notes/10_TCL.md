# Transaction Control Language (TCL) in SQL

## 1. TCL이란?
- **TCL(트랜잭션 제어 언어)**은 데이터베이스 내에서 일련의 작업(트랜잭션)을 **원자성**, **일관성**, **격리성**, **지속성**(ACID) 보장 하에 실행·관리하는 명령어들입니다.  
- 여러 DML(INSERT/UPDATE/DELETE) 작업을 하나의 단위로 묶어, 모두 성공하거나 모두 취소되게 합니다.

---

## 2. 트랜잭션의 ACID 특성
| 특성           | 설명                                                                                  |
|---------------|---------------------------------------------------------------------------------------|
| **A (Atomicity)**   | 트랜잭션 내 모든 작업이 **모두 실행**되거나, **하나도 실행되지 않아야** 함             |
| **C (Consistency)** | 트랜잭션 실행 전후 데이터의 **무결성 제약**이 항상 유지되어야 함                        |
| **I (Isolation)**   | **동시성 제어**를 통해 트랜잭션 간 간섭 방지; 각 트랜잭션은 독립적으로 실행된 것처럼 보임 |
| **D (Durability)**  | 커밋된 트랜잭션의 결과는 **영구 저장**되어 시스템 오류에도 보존됨                     |

---

## 3. 주요 TCL 명령어

### 3.1 `START TRANSACTION` / `BEGIN`
- 트랜잭션을 **명시적으로 시작**  
```sql
START TRANSACTION;
-- 또는
BEGIN;
```

### 3.2 `COMMIT`
- 트랜잭션 내의 모든 변경을 **영구 반영**  
```sql
COMMIT;
```

### 3.3 `ROLLBACK`
- 트랜잭션 내에서 발생한 모든 변경을 **취소(되돌리기)**  
```sql
ROLLBACK;
```

### 3.4 `SAVEPOINT`
- 트랜잭션 처리 중 **중간 지점**(저장점)을 설정  
```sql
SAVEPOINT sp_before_update;
```

### 3.5 `ROLLBACK TO SAVEPOINT`
- 지정한 저장점까지 **되돌리기**, 저장점 이후 작업만 취소  
```sql
ROLLBACK TO SAVEPOINT sp_before_update;
```

### 3.6 `RELEASE SAVEPOINT`
- 더 이상 필요 없는 저장점을 **해제(제거)**  
```sql
RELEASE SAVEPOINT sp_before_update;
```

---

## 4. 사용 예시

```sql
-- 트랜잭션 시작
START TRANSACTION;

-- 주문 테이블에 새 주문 삽입
INSERT INTO orders (id, customer_id, amount)
VALUES (101, 1, 250.00);

-- 재고 테이블 수량 업데이트
UPDATE inventory
SET quantity = quantity - 1
WHERE product_id = 55;

-- 중간 저장점 설정
SAVEPOINT sp_after_update;

-- 결제 정보 삽입 (실패 가능성 있는 작업)
INSERT INTO payments (order_id, paid_amount)
VALUES (101, 250.00);

-- 만약 결제 정보 삽입 중 오류 발생 시
-- ROLLBACK TO sp_after_update;

-- 모두 정상이라면
COMMIT;
```

---

## 5. 주의사항 & 팁

- MySQL의 기본 모드는 **자동 커밋(auto‐commit)** 이므로, 별도 `START TRANSACTION` 없이 실행된 DML은 즉시 커밋됩니다.  
- **수동 트랜잭션**을 사용하려면 `START TRANSACTION` 후 `COMMIT` 또는 `ROLLBACK`을 반드시 호출해야 합니다.  
- **저장점(SAVEPOINT)** 을 적극 활용하면 복잡한 트랜잭션 내에서도 부분 취소가 가능합니다.  
- 트랜잭션이 길어지면 **락(lock)** 대기가 증가하므로, 가능한 **짧게 유지**하는 것이 성능에 유리합니다.

---

## 6. 요약

- **TCL**: `START TRANSACTION` → DML 작업들 → `COMMIT` 또는 `ROLLBACK`  
- **SAVEPOINT**로 중간 지점 설정 후 선택적 롤백 가능  
- 트랜잭션 관리는 **데이터 무결성**과 **동시성 성능**을 위해 필수  