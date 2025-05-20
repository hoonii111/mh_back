# 📌 MySQL Index 정리

MySQL에서 **인덱스(Index)**는 데이터 검색 속도를 향상시키기 위한 핵심 기능임. 
특히 대용량 데이터베이스에서 성능 개선에 매우 중요

---

## 1. 인덱스란?

- **테이블에서 원하는 데이터를 빠르게 찾을 수 있게 해주는 구조**  
- 책의 목차와 비슷한 역할을 수행
- 주로 `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY` 등에 사용됨

---

## 2. 인덱스의 장단점

### ✅ 장점
- 검색 속도 향상 (`SELECT`)
- 정렬, 필터링, 조인 성능 향상
- `UNIQUE` 인덱스를 통한 중복 데이터 방지

### ⚠️ 단점
- `INSERT`, `UPDATE`, `DELETE` 시 오히려 성능 저하 가능 (인덱스 갱신 필요)
- 디스크 공간 추가 소요
- 과도한 인덱스는 오히려 성능 저하

---

## 3. 인덱스 종류

| 인덱스 종류        | 설명 |
|---------------------|------|
| **PRIMARY KEY**     | 기본 키 인덱스. 자동으로 클러스터형 인덱스 생성 |
| **UNIQUE INDEX**    | 중복 불가. 고유한 값을 유지 |
| **INDEX** (일반 인덱스) | 중복 허용. 가장 일반적인 인덱스 |
| **FULLTEXT INDEX**  | 텍스트 기반 검색용 (`VARCHAR`, `TEXT`) |
| **SPATIAL INDEX**   | 공간(GIS) 데이터용 인덱스

---

## 4. 인덱스 생성 & 삭제 문법

### 🔹 인덱스 생성
```sql
-- 일반 인덱스
CREATE INDEX idx_username ON users(username);

-- 유니크 인덱스
CREATE UNIQUE INDEX idx_email ON users(email);

-- 풀텍스트 인덱스
CREATE FULLTEXT INDEX idx_content ON articles(content);
```

### 🔹 인덱스 삭제
```sql
DROP INDEX idx_username ON users;
```

> 🔎 `PRIMARY KEY` 또는 `UNIQUE`는 테이블 생성 시에도 설정 가능

---

## 5. 인덱스 확인 및 분석

### 🔹 현재 테이블 인덱스 조회
```sql
SHOW INDEX FROM users;
```

### 🔹 인덱스 사용 확인 (실행 계획)
```sql
EXPLAIN SELECT * FROM users WHERE username = 'alice';
```

---

## 6. 인덱스 사용이 효과적인 경우

- `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY` 등에 자주 쓰이는 칼럼
- 자주 검색되는 문자열/숫자 칼럼
- 고유성 보장이 필요한 경우 (`UNIQUE`)
- 외래 키 관계 설정 시 (`FOREIGN KEY`)

---

## 7. 인덱스 설계 팁

- **너무 많은 인덱스는 금물**: 쓰기 성능 저하 우려
- **복합 인덱스**는 자주 같이 검색되는 칼럼에 유리
- **조회 빈도**가 높은 칼럼 위주로 적용
- **카디널리티(값의 다양성)**가 높은 칼럼에 효과적

---

## ✅ 정리 요약

- 인덱스는 성능 최적화의 핵심
- 조회는 빨라지지만, 삽입/수정/삭제는 느려질 수 있음
- 잘 설계된 인덱스는 대용량 데이터 처리 속도를 획기적으로 개선