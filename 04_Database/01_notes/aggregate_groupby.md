# Aggregate Functions & GROUP BY in SQL

## 1. 집계 함수(Aggregate Functions)란?
- 여러 행의 값을 하나의 결과로 요약할 때 사용  
- 대표 함수:  
  - `COUNT()` : 행 개수  
  - `SUM()`   : 합계  
  - `AVG()`   : 평균  
  - `MIN()`   : 최소값  
  - `MAX()`   : 최대값  

---

## 2. 기본 사용 예시

```sql
-- 전체 주문 건수
SELECT COUNT(*) AS total_orders
FROM orders;

-- 전체 매출 합계
SELECT SUM(order_amount) AS total_sales
FROM orders;

-- 주문 금액 평균
SELECT AVG(order_amount) AS avg_order
FROM orders;

-- 최소/최대 주문 금액
SELECT MIN(order_amount) AS min_order,
       MAX(order_amount) AS max_order
FROM orders;
```

---

## 3. GROUP BY

- 특정 컬럼별로 그룹을 나눠 **그룹 단위 집계**를 수행  
- `SELECT` 절에 그룹화 대상 컬럼 + 집계 함수 함께 사용

### 사용 예시

```sql
-- 고객별 주문 합계
SELECT customer_id,
       SUM(order_amount) AS total_amount
FROM orders
GROUP BY customer_id;

-- 월별 매출 합계 (order_date가 DATE 타입)
SELECT MONTH(order_date) AS month,
       SUM(order_amount) AS monthly_sales
FROM orders
GROUP BY MONTH(order_date);
```

---

## 4. HAVING

- `WHERE`는 그룹화 전 **행 단위 필터**  
- `HAVING`은 그룹화 후 **집계 결과 필터**  
- `HAVING` 절에서 집계 함수를 사용할 수 있음

### 사용 예시

```sql
-- 고객별 매출이 1000 이상인 고객만 조회
SELECT customer_id,
       SUM(order_amount) AS total_amount
FROM orders
GROUP BY customer_id
HAVING SUM(order_amount) >= 1000;
```

---

## 5. ROLLUP & CUBE (선택)

- **ROLLUP**: 상위 집계 추가 (소계 → 총계)  
- **CUBE**: 다차원 집계(모든 조합의 소계 및 총계)

### ROLLUP 예시

```sql
SELECT customer_id,
       product_category,
       SUM(order_amount) AS sum_amount
FROM orders
GROUP BY customer_id, product_category WITH ROLLUP;
```

> 결과 맨 아래에 `customer_id=NULL` 또는 `product_category=NULL` 로 소계/총계 표시

---

## 6. 실습 팁

1. **GROUP BY** 없이 집계 함수만 사용하면 한 줄 결과  
2. **NULL** 값 그룹화 주의 (`NULL`은 별도 그룹)  
3. **ORDER BY** 와 함께 쓰면 정렬된 집계 결과 가능  
   ```sql
   SELECT customer_id, SUM(order_amount) AS total
   FROM orders
   GROUP BY customer_id
   ORDER BY total DESC;
   ```

---

## 7. 요약

- `COUNT`, `SUM`, `AVG`, `MIN`, `MAX` 로 **데이터 요약**  
- `GROUP BY` 로 **그룹 단위 집계**  
- `HAVING` 으로 **집계 결과 필터**  
- `ROLLUP`, `CUBE` 로 **소계/총계** 확장 가능  