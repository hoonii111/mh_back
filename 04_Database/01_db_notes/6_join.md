# JOIN in SQL

## 1. JOIN이란?

JOIN은 두 개 이상의 테이블을 **공통된 컬럼(기준값)**을 기준으로 연결하여, **연관된 데이터를 함께 조회**할 수 있게 해주는 SQL 문법입니다.

- 일반적으로 `외래키(foreign key)`를 기준으로 테이블을 연결
- `ON` 또는 `USING` 키워드로 조인 조건을 명시

---

## 2. JOIN의 종류

### 2.1 INNER JOIN (내부 조인)

- 두 테이블 간 **일치하는 값이 있는 행만** 조회  
```sql
SELECT *
FROM orders
INNER JOIN customers
ON orders.customer_id = customers.id;
```

> ✅ 일치하지 않으면 결과에 포함되지 않음

---

### 2.2 LEFT JOIN (또는 LEFT OUTER JOIN)

- 왼쪽 테이블은 **모두 표시**, 오른쪽 테이블은 일치할 경우에만 값 표시  
```sql
SELECT *
FROM customers
LEFT JOIN orders
ON customers.id = orders.customer_id;
```

> ✅ 주문이 없는 고객도 조회됨 (주문 정보는 NULL)

---

### 2.3 RIGHT JOIN (또는 RIGHT OUTER JOIN)

- 오른쪽 테이블은 **모두 표시**, 왼쪽 테이블은 일치할 경우에만 값 표시  
```sql
SELECT *
FROM orders
RIGHT JOIN customers
ON orders.customer_id = customers.id;
```

> ✅ 주문이 없는 고객을 포함하려면 LEFT JOIN으로 방향 조정 가능

---

### 2.4 FULL OUTER JOIN (모든 데이터 포함)

- **양쪽 테이블 모두 포함**, 일치하지 않는 데이터는 NULL로 채움  
```sql
-- MySQL은 FULL OUTER JOIN 미지원 → UNION으로 구현
SELECT *
FROM customers
LEFT JOIN orders ON customers.id = orders.customer_id
UNION
SELECT *
FROM customers
RIGHT JOIN orders ON customers.id = orders.customer_id;
```

---

### 2.5 SELF JOIN (자기 자신과 조인)

- 같은 테이블을 서로 다른 이름(alias)으로 **자체 조인**  
```sql
SELECT a.name AS employee, b.name AS manager
FROM employees a
JOIN employees b
ON a.manager_id = b.id;
```

---

### 2.6 CROSS JOIN (카테시안 곱)

- 조인 조건 없이 **모든 조합을 생성**  
```sql
SELECT *
FROM students
CROSS JOIN subjects;
```

> ✅ 학생 10명 × 과목 3개 → 30개의 결과 생성

---

## 3. JOIN vs SUBQUERY

| 비교 항목 | JOIN | SUBQUERY |
|-----------|------|----------|
| 읽기 성능 | 빠른 편 | 느릴 수 있음 |
| 복잡도 | 낮음 (간결함) | 높아질 수 있음 |
| 필터링 방식 | 여러 테이블을 동시에 조작 | 내부 쿼리로 중첩 조작 |
| 사용 목적 | 관련된 행을 나란히 조회 | 조건에 따른 필터링이나 집계 |

---

## 4. 실습 예시

### 테이블 구조

```sql
-- customers 테이블
id | name
---|------
1  | Alice
2  | Bob
3  | Charlie

-- orders 테이블
id | customer_id | product
---|-------------|--------
1  | 1           | Book
2  | 2           | Pen
3  | 1           | Laptop
```

### INNER JOIN 예시
```sql
SELECT c.name, o.product
FROM customers c
JOIN orders o
ON c.id = o.customer_id;
```

> 💡 결과: Alice - Book, Bob - Pen, Alice - Laptop

---

## 5. 참고 사항

- 조인 조건이 없으면 **데이터 폭발(Cartesian Product)** 위험 있음
- `ON`은 **테이블 간 조건**, `WHERE`은 **조회 조건**
- JOIN 시 **컬럼명이 겹치면 alias 사용** 권장

---

## 📌 요약

- **INNER JOIN**: 공통 데이터만
- **LEFT/RIGHT JOIN**: 한쪽 테이블은 전부 포함
- **FULL JOIN**: 양쪽 테이블 모두 포함 (MySQL은 직접 구현 필요)
- **SELF JOIN**: 자기 자신과 조인
- **CROSS JOIN**: 조합 생성 (카테시안 곱)