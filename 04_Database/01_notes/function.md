# SQL Functions

MySQL에서 자주 사용하는 함수(Function)를 **범주별**로 정리한 문서입니다.  
문자열, 수치, 날짜/시간 함수와 함께 간단한 예제를 포함하고 있습니다.

---

## 1. 문자열 함수 (String Functions)

| 함수                    | 설명                                           | 예제                                                        |
|-------------------------|------------------------------------------------|-------------------------------------------------------------|
| `CONCAT(str1, str2, …)` | 여러 문자열을 이어 붙임                        | `SELECT CONCAT(first_name, ' ', last_name) FROM users;`     |
| `CONCAT_WS(sep, …)`     | 구분자(sep)로 여러 문자열을 이어 붙임          | `SELECT CONCAT_WS(',', city, state) FROM addresses;`        |
| `LENGTH(str)`           | 바이트 단위 문자열 길이 반환                   | `SELECT LENGTH('안녕');           -- 결과: 6`                 |
| `CHAR_LENGTH(str)`      | 문자 단위 문자열 길이 반환                     | `SELECT CHAR_LENGTH('안녕');      -- 결과: 2`                 |
| `LOWER(str)`            | 모든 영문자를 소문자로 변환                    | `SELECT LOWER('Hello');           -- 결과: 'hello'`          |
| `UPPER(str)`            | 모든 영문자를 대문자로 변환                    | `SELECT UPPER('Hello');           -- 결과: 'HELLO'`          |
| `SUBSTR(str, pos, len)` | 문자열의 일부분(substring) 반환                | `SELECT SUBSTR('abcdef', 2, 3);    -- 결과: 'bcd'`            |
| `REPLACE(str, from, to)`| 문자열 내 일부 문자열을 다른 문자열로 치환     | `SELECT REPLACE('2025-05-22','-','/');  -- '2025/05/22'`     |
| `TRIM(str)`             | 문자열 양쪽 공백 제거                           | `SELECT TRIM('  hello  ');         -- 'hello'`               |

---

## 2. 수치 함수 (Numeric Functions)

| 함수                       | 설명                                             | 예제                                                      |
|----------------------------|--------------------------------------------------|-----------------------------------------------------------|
| `ROUND(x, d)`              | 소수점 d자리까지 반올림                          | `SELECT ROUND(123.4567, 2);   -- 123.46`                   |
| `FLOOR(x)`                 | 소수점 이하 버림                                  | `SELECT FLOOR(123.9);         -- 123`                      |
| `CEIL(x)` / `CEILING(x)`   | 소수점 올림                                      | `SELECT CEIL(123.1);          -- 124`                      |
| `ABS(x)`                   | 절대값                                           | `SELECT ABS(-45.7);           -- 45.7`                     |
| `MOD(x, y)`                | 나머지                                           | `SELECT MOD(10, 3);           -- 1`                        |
| `POWER(x, y)`              | x<sup>y</sup> 제곱                               | `SELECT POWER(2, 3);          -- 8`                        |
| `SQRT(x)`                  | 제곱근                                           | `SELECT SQRT(16);             -- 4`                        |
| `CEILING(x)`               | 올림 (CEIL과 동일)                              | `SELECT CEILING(2.3);         -- 3`                        |

---

## 3. 날짜/시간 함수 (Date & Time Functions)

| 함수                                    | 설명                                                    | 예제                                                      |
|-----------------------------------------|---------------------------------------------------------|-----------------------------------------------------------|
| `NOW()`                                 | 현재 날짜와 시간 반환                                    | `SELECT NOW();        -- '2025-05-22 14:23:45'`            |
| `CURDATE()` / `CURRENT_DATE()`          | 현재 날짜(YYYY-MM-DD)                                    | `SELECT CURDATE();    -- '2025-05-22'`                     |
| `CURTIME()` / `CURRENT_TIME()`          | 현재 시간(HH:MM:SS)                                     | `SELECT CURTIME();    -- '14:23:45'`                       |
| `DATE(expr)`                            | `DATETIME`/`TIMESTAMP`에서 날짜 부분만 추출              | `SELECT DATE(NOW());  -- '2025-05-22'`                     |
| `TIME(expr)`                            | `DATETIME`/`TIMESTAMP`에서 시간 부분만 추출              | `SELECT TIME(NOW());  -- '14:23:45'`                       |
| `DATE_FORMAT(date, format)`             | 지정한 형식(format)으로 날짜 문자열 반환                 | `SELECT DATE_FORMAT(NOW(), '%Y/%m/%d %H:%i');`            |
| `DATEDIFF(date1, date2)`                | 두 날짜의 차이(일수)                                    | `SELECT DATEDIFF('2025-05-22','2025-05-01');  -- 21`      |
| `TIMESTAMPDIFF(unit, dt1, dt2)`         | 단위(unit)별 두 `DATETIME` 차이 반환                     | `SELECT TIMESTAMPDIFF(HOUR, '2025-05-22 00:00','2025-05-22 12:00'); -- 12` |
| `ADDDATE(date, INTERVAL expr unit)`     | 날짜에 지정한 간격(expr unit) 더하기                     | `SELECT ADDDATE(CURDATE(), INTERVAL 7 DAY);   -- +7일 후 날짜` |
| `SUBDATE(date, INTERVAL expr unit)`     | 날짜에서 지정한 간격(expr unit) 빼기                     | `SELECT SUBDATE(CURDATE(), INTERVAL 1 MONTH); -- 1개월 전 날짜` |

---

## 4. 제어 흐름 함수 (Control Flow Functions)

| 함수                       | 설명                                                      | 예제                                                      |
|----------------------------|-----------------------------------------------------------|-----------------------------------------------------------|
| `IF(expr, true_val, false_val)`    | 조건(expr)이 참이면 `true_val`, 거짓이면 `false_val` 반환 | `SELECT IF(score>=60, 'PASS', 'FAIL') FROM exams;`        |
| `IFNULL(expr, alt_val)`    | `expr`이 `NULL`이면 `alt_val` 반환                         | `SELECT IFNULL(memo, 'N/A') FROM notes;`                  |
| `COALESCE(val1, val2, …)`  | NULL이 아닌 첫 번째 값 반환                                | `SELECT COALESCE(NULL, NULL, 'Hello', 'World');  -- 'Hello'` |
| `CASE WHEN … THEN … ELSE … END` | 복잡한 조건 분기 처리                                     | ```sql
CASE
  WHEN age < 18 THEN 'Minor'
  WHEN age BETWEEN 18 AND 64 THEN 'Adult'
  ELSE 'Senior'
END AS age_group
``` |

---

## 5. 요약

- **문자열 함수**: `CONCAT`, `SUBSTR`, `REPLACE` 등으로 텍스트 처리  
- **수치 함수**: `ROUND`, `FLOOR`, `CEIL` 등 수학적 계산  
- **날짜/시간 함수**: `NOW`, `DATE_FORMAT`, `DATEDIFF` 등 시간 연산  
- **제어 흐름**: `IF`, `CASE`, `COALESCE` 등 조건 분기  

