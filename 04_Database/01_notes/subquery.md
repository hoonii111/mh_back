# Subquery in SQL

## 1. 서브쿼리(Subquery)란?
- **쿼리 안에 포함된 또 다른 쿼리**  
- 메인 쿼리의 결과나 조건에 서브쿼리가 사용됨  
- 가독성, 재사용성, 복잡한 로직 처리에 유용

---

## 2. 서브쿼리 종류

| 구분               | 설명                                                     | 위치 예시           |
|--------------------|----------------------------------------------------------|---------------------|
| **스칼라 서브쿼리**  | 반환값이 **1행 1열**인 경우<br>단일 값으로 비교하거나 대입  | `SELECT`, `WHERE`   |
| **컬럼(행) 서브쿼리** | 반환값이 **1행 다열**인 경우<br>여러 컬럼을 한 번에 조회     | `SELECT`            |
| **테이블 서브쿼리**  | 반환값이 **다행 다열**인 경우<br>FROM 절에 가상 테이블로 사용 | `FROM`, `WHERE IN`  |

---

## 3. 위치별 사용 예시

### 3.1 WHERE절 서브쿼리

```sql
-- orders 테이블에서 가장 큰 order_amount를 가진 주문 조회
SELECT *
FROM orders
WHERE order_amount = (
    SELECT MAX(order_amount)
    FROM orders
);
```

### 3.2 FROM절 서브쿼리 (Derived Table)

```sql
-- 고객별 주문 합계를 구한 뒤, 총액이 큰 순으로 조회
SELECT c.customer_id, c.total_amount
FROM (
    SELECT customer_id, SUM(order_amount) AS total_amount
    FROM orders
    GROUP BY customer_id
) AS c
ORDER BY c.total_amount DESC;
```

### 3.3 SELECT절 서브쿼리

```sql
-- 각 주문에 대해 해당 고객의 총 주문 횟수를 함께 조회
SELECT 
    o.order_id,
    o.customer_id,
    o.order_amount,
    (
        SELECT COUNT(*)
        FROM orders
        WHERE customer_id = o.customer_id
    ) AS order_count
FROM orders AS o;
```

---

## 4. IN / EXISTS vs JOIN

| 연산자             | 특징                                                          | 예시                                                     |
|--------------------|---------------------------------------------------------------|----------------------------------------------------------|
| **IN (값 목록)**    | 서브쿼리 결과가 **여러 값**일 때 사용                           | `WHERE customer_id IN (SELECT id FROM vip_customers)`    |
| **EXISTS**         | 서브쿼리 결과가 **행 존재 여부**만 검사                         | `WHERE EXISTS (SELECT 1 FROM reviews r WHERE r.user_id = u.id)` |
| **JOIN**           | 두 테이블을 **직접 연결**하여 조회<br>복잡한 서브쿼리 대체 가능   | `FROM orders o JOIN customers c ON o.customer_id = c.id` |

---

## 5. 성능 팁

- **서브쿼리 과도 사용 주의**: 복잡한 서브쿼리가 많은 경우 성능 저하  
- **JOIN 대체 검토**: 경우에 따라 `JOIN`이 더 효율적  
- **인덱스 활용**: 서브쿼리 조건 컬럼에 인덱스 설정 권장  
- **EXISTS vs IN**: 대규모 데이터셋에선 `EXISTS`가 더 빠를 수 있음

---

## 6. 요약

- 서브쿼리는 **다양한 위치**에서 사용 가능 (`SELECT`, `FROM`, `WHERE`)  
- 반환 형태에 따라 **스칼라 / 컬럼 / 테이블 서브쿼리** 구분  
- **JOIN** 또는 **IN/EXISTS** 와의 차이점 이해 중요  
- **성능**을 고려해 적절히 사용