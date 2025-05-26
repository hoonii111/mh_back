# View in SQL

## 1. 뷰(View)란?

- **뷰(View)** 는 하나 이상의 테이블을 조합한 **가상 테이블**입니다.  
- 실제 데이터가 저장되지 않고, 뷰 정의 시 작성한 SELECT 문이 실행될 때마다 결과가 조회됩니다.  
- 복잡한 쿼리를 **단순화** 하고, **보안**을 강화하며, 재사용성을 높입니다.


---

## 2. 뷰의 장점

- **간편성** : 복잡한 JOIN/집계 쿼리를 미리 정의해 놓고, 마치 테이블처럼 조회  
- **보안** : 민감한 컬럼을 제외한 뷰만 사용자에게 제공 가능  
- **유지 보수** : 쿼리 로직 변경이 필요할 때 뷰 정의만 수정  
- **재사용성** : 여러 곳에서 동일한 뷰를 참조

---

## 3. 뷰 생성 및 관리

### 3.1 뷰 생성

```sql
CREATE VIEW vip_customers AS
SELECT
    c.id,
    c.name,
    SUM(o.order_amount) AS total_spent
FROM
    customers c
    JOIN orders o ON c.id = o.customer_id
GROUP BY
    c.id, c.name
HAVING
    SUM(o.order_amount) > 1000;
```

- `vip_customers` 뷰는 총 구매액이 1,000보다 큰 고객만 조회

---

### 3.2 뷰 조회

```sql
SELECT * 
FROM vip_customers;
```

- 실제 테이블이 아니더라도, 마치 테이블처럼 간단히 조회 가능

---

### 3.3 뷰 수정

```sql
CREATE OR REPLACE VIEW vip_customers AS
SELECT
    c.id,
    c.name,
    COUNT(o.id) AS order_count,
    SUM(o.order_amount) AS total_spent
FROM
    customers c
    JOIN orders o ON c.id = o.customer_id
GROUP BY
    c.id, c.name
HAVING
    COUNT(o.id) >= 5;
```

- `OR REPLACE` 옵션으로 기존 뷰를 수정

---

### 3.4 뷰 삭제

```sql
DROP VIEW IF EXISTS vip_customers;
```

- 존재하지 않는 뷰도 에러 없이 삭제

---

## 4. 뷰와 테이블의 차이

| 항목             | 테이블(Table)        | 뷰(View)                            |
|------------------|----------------------|-------------------------------------|
| 데이터 저장      | 실제 로우를 저장     | 데이터는 저장되지 않고 정의된 쿼리 결과로 조회 |
| 업데이트 가능 여부 | 가능 (INSERT/UPDATE/DELETE) | 대부분 읽기 전용 (단일 테이블 단순 뷰만 일부 수정 가능) |
| 성능             | 직접 저장된 데이터 읽기 | 매번 정의 쿼리 실행하여 오버헤드 발생 가능 |

---

## 5. 뷰 활용 팁

- **보안 필터링** : 민감 정보(예: 급여, 주민번호) 필터링된 뷰 제공  
- **비즈니스 로직 캡슐화** : 복잡한 집계나 파생 컬럼 계산 로직을 뷰에 숨김  
- **논리적 스키마 분리** : 동일 테이블을 서로 다른 관점으로 제공 (예: 지역별 뷰, 기간별 뷰)  

---

## 6. 주의사항

- **성능 오버헤드** : 뷰를 많이 중첩하거나 복잡한 뷰는 실행 속도 저하  
- **업데이트 제약** : 조인된 뷰나 집계 뷰는 기본적으로 쓰기(DML) 불가  
- **의존성 관리** : 뷰에 참조된 테이블 구조 변경 시 뷰가 깨질 수 있음  

---

## 7. 요약

- **view**는 재사용성과 보안을 위해 복잡한 쿼리를 가상 테이블로 추상화  
- **생성**: `CREATE VIEW … AS SELECT …`  
- **수정**: `CREATE OR REPLACE VIEW`  
- **삭제**: `DROP VIEW`  
- **단점**: 성능 및 업데이트 제한 고려 필요  