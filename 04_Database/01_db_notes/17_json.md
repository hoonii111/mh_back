# JSON 데이터 타입 & 함수 (MySQL)

MySQL 5.7 이상에서 지원하는 JSON 컬럼과 관련 함수를 정리함.

---

## 1. JSON 데이터 타입

```sql
CREATE TABLE events (
    id INT PRIMARY KEY AUTO_INCREMENT,
    payload JSON NOT NULL
);
```

* **JSON 컬럼**은 유효한 JSON 문서만 저장
* 내부적으로 최적화된 바이너리 형식으로 저장되어 효율적

---

## 2. 주요 JSON 함수

| 함수                                   | 설명                              | 예시                                                           |
| ------------------------------------ | ------------------------------- | ------------------------------------------------------------ |
| `JSON_EXTRACT(json, path)`           | JSON에서 경로(`path`)에 해당하는 값을 반환   | `SELECT JSON_EXTRACT(payload, '$.user.name') FROM events;`   |
| `->` / `->>` 연산자                     | `->`는 JSON 값, `->>`는 텍스트로 반환    | `payload->'$.user'`, `payload->>'$.user.name'`               |
| `JSON_SET(json, path, val)`          | JSON에 경로를 추가 또는 갱신              | `UPDATE events SET payload = JSON_SET(payload, '$.x', 10);`  |
| `JSON_REMOVE(json, path)`            | JSON에서 지정 경로의 요소를 제거            | `UPDATE events SET payload = JSON_REMOVE(payload, '$.tmp');` |
| `JSON_ARRAY(val, …)`                 | JSON 배열 생성                      | `SELECT JSON_ARRAY(1, 2, 3);`                                |
| `JSON_OBJECT(key, val, …)`           | 키-값 쌍으로 JSON 객체 생성              | `SELECT JSON_OBJECT('a',1,'b',2);`                           |
| `JSON_SEARCH(json, one_or_all, val)` | 지정 값(`val`)이 존재하는 경로 검색         | `SELECT JSON_SEARCH(payload, 'one', 'Alice');`               |
| `JSON_CONTAINS(json, val, path)`     | JSON 내부에 지정 값(`val`)이 포함되었는지 검사 | `SELECT JSON_CONTAINS(payload, '"Alice"', '$.users');`       |
| `JSON_UNQUOTE(json_val)`             | 이스케이프된 JSON 문자열에서 따옴표 제거        | `SELECT JSON_UNQUOTE(JSON_EXTRACT(payload, '$.user.name'));` |

---

## 3. JSON 인덱싱

* \*\*가상 컬럼(Virtual Generated Column)\*\*과 인덱스를 결합하여 JSON 필드를 인덱싱할 수 있음

```sql
ALTER TABLE events
ADD COLUMN user_name VARCHAR(50) 
    GENERATED ALWAYS AS (JSON_UNQUOTE(payload->>'$.user.name')) VIRTUAL,
ADD INDEX idx_user_name (user_name);
```

* 실질적인 데이터 저장 없이 인덱스만 유지됨.

---

## 4. 활용 사례

* **로그/이벤트 저장소**: 다양한 속성의 이벤트 데이터를 유연하게 저장
* **설정 데이터**: 애플리케이션 설정을 JSON 형식으로 저장
* **반정규화 데이터**: JOIN 없이 한 번에 조회하기 위해 JSON 사용

---

## 5. 주의사항

* JSON 경로 표현식(`$.field.subfield`)을 정확히 지정해야 오류를 방지
* 빈번한 업데이트가 필요한 JSON 컬럼은 성능 저하 우려가 있으므로 신중히 설계
* 대량의 JSON 데이터를 처리할 경우, 정규화된 테이블과 조합하여 사용하는 것이 좋음.

---

## 6. 참고 자료

* MySQL 공식 문서: [JSON Functions](https://dev.mysql.com/doc/refman/8.0/en/json-function-reference.html)