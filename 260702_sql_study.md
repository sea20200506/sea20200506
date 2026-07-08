# MySQL 기초 - 수치/문자열/날짜 연산과 CASE 문

**2026.07.02**  
`MySQL` `SQL` `연산자` `함수` `CASE` `ROUND` `CONCAT` `날짜연산`

---

## 1. 사칙연산

### 연산자 우선순위

| 우선순위 | 연산자 |
|---|---|
| 1순위 | `*` `/` `%` |
| 2순위 | `+` `-` |

기본적으로 왼쪽에서 오른쪽 순서로 처리.

---

## 2. SELECT 구에서 연산

```sql
SELECT *, price*quantity FROM sample34;
```

| no | price | quantity | price*quantity |
|---|---|---|---|
| 1 | 100 | 10 | 1000 |
| 2 | 230 | 24 | 5520 |
| 3 | 1980 | 1 | 1980 |

---

## 3. 별명 (AS)

```sql
SELECT *, price*quantity AS amount FROM sample34;
```

| no | price | quantity | amount |
|---|---|---|---|
| 1 | 100 | 10 | 1000 |
| 2 | 230 | 24 | 5520 |
| 3 | 1980 | 1 | 1980 |

- `AS` 생략 가능: `SELECT price*quantity amount`
- 별명은 영어, 숫자, 한글 모두 가능
- 한글 별명은 백쿼트(MySQL) 또는 더블쿼트로 감쌀 것
- 예약어를 별명으로 쓰려면 더블쿼트로 감쌀 것: `AS "SELECT"`
- 객체명은 숫자로 시작 불가 (더블쿼트로 감싸면 예외적으로 허용, 단 숫자만으로 구성된 이름은 불가)

> 더블쿼트 → DB 객체명으로 간주  
> 싱글쿼트 → 문자열 상수

---

## 4. WHERE 구에서 연산

```sql
SELECT *, price*quantity AS amount FROM sample34 WHERE price*quantity >= 2000;
```

| no | price | quantity | amount |
|---|---|---|---|
| 2 | 230 | 24 | 5520 |

```sql
-- 에러: WHERE 구에서 별명 사용 불가
SELECT *, price*quantity AS amount FROM sample34 WHERE amount >= 2000;
-- ERROR: Unknown column 'amount' in 'where clause'
```

### 내부 처리 순서

```
WHERE 구 → SELECT 구 → ORDER BY 구
```

WHERE가 SELECT보다 먼저 처리되므로, SELECT에서 붙인 별명은 WHERE에서 아직 존재하지 않음.  
ORDER BY는 가장 나중에 처리되므로 SELECT의 별명을 사용할 수 있음.

```sql
-- ORDER BY에서 별명 사용 가능
SELECT *, price*quantity AS amount FROM sample34 ORDER BY amount DESC;
```

| no | price | quantity | amount |
|---|---|---|---|
| 2 | 230 | 24 | 5520 |
| 3 | 1980 | 1 | 1980 |
| 1 | 100 | 10 | 1000 |

---

## 5. NULL 값의 연산

NULL이 포함된 연산은 결과가 항상 NULL.

```
NULL + 1  → NULL
1 / NULL  → NULL (에러 아님)
```

---

## 6. 함수

- 연산자: 기호로 연산 방법 결정 (`+`, `-` 등)
- 함수: 함수명으로 연산 방법 결정, 인수를 괄호로 지정
- 인수 = 파라미터, 함수의 결괏값 = 반환값

### 나머지 연산

| 방법 | 문법 | 지원 DB |
|---|---|---|
| `%` 연산자 | `10 % 3` | MySQL 등 |
| `MOD` 함수 | `MOD(10, 3)` | MySQL, Oracle 등 |

---

## 7. ROUND 함수 - 반올림

```sql
SELECT amount, ROUND(amount) FROM sample341;
```

| amount | ROUND(amount) |
|---|---|
| 5961.60 | 5962 |
| 2138.40 | 2138 |
| 1080.00 | 1080 |

### 두 번째 인수로 자릿수 지정

| 두 번째 인수 | 동작 |
|---|---|
| 생략 or `0` | 소수점 첫째 자리에서 반올림 |
| `1` | 소수점 둘째 자리에서 반올림 |
| `-1` | 1단위 반올림 |
| `-2` | 10단위 반올림 |

```sql
SELECT amount, ROUND(amount, 1) FROM sample341;
```

| amount | ROUND(amount, 1) |
|---|---|
| 5961.60 | 5961.6 |
| 2138.40 | 2138.4 |
| 1080.00 | 1080.0 |

```sql
SELECT amount, ROUND(amount, -2) FROM sample341;
```

| amount | ROUND(amount, -2) |
|---|---|
| 5961.60 | 6000 |
| 2138.40 | 2100 |
| 1080.00 | 1100 |

> 버림은 `TRUNCATE` 함수 사용.

---

## 8. 문자열 연산

### 문자열 결합

DB마다 문법이 다름.

| DB | 문법 |
|---|---|
| MySQL | `CONCAT(a, b)` |
| Oracle, PostgreSQL | `a \|\| b` |
| SQL Server | `a + b` |

```sql
SELECT CONCAT(quantity, unit) FROM sample35;
```

| CONCAT(quantity, unit) |
|---|
| 10개 |
| 24통 |
| 1장 |

수치형 데이터도 CONCAT으로 결합 가능. 결과는 문자열형으로 반환됨.

### SUBSTRING 함수 - 문자열 일부 추출

```sql
SUBSTRING('20140125001', 1, 4)  -- '2014' (1번째 자리부터 4글자)
SUBSTRING('20140125001', 5, 2)  -- '01'   (5번째 자리부터 2글자)
```

DB에 따라 `SUBSTR`로 사용하기도 함.

### TRIM 함수 - 앞뒤 공백 제거

```sql
TRIM('ABC   ')  -- 'ABC'
```

- 문자열 도중의 공백은 제거하지 않음
- 주로 CHAR형(고정 길이)에서 남은 공백 제거 시 사용
- 인수 지정으로 공백 외 문자도 제거 가능

### CHARACTER_LENGTH / CHAR_LENGTH 함수 - 문자열 길이

- `CHAR_LENGTH`: 문자 수 기준
- `OCTET_LENGTH`: 바이트 수 기준 → 문자세트에 따라 결과 다름

### 문자세트별 바이트 수 비교

문자열 'A는 반각, 한은 전각' 기준:

| 문자세트 | 문자 수 | 바이트 수 |
|---|---|---|
| EUC-KR | 12 | 19 |
| UTF-8 | 12 | 26 |

- EUC-KR: ASCII 1바이트, 한글 2바이트
- UTF-8: ASCII 1바이트, 한글 3바이트

> CHAR_LENGTH는 문자 수로 계산하므로 문자세트 영향 없음.  
> OCTET_LENGTH는 바이트 기준이므로 문자세트 주의 필요.

---

## 9. 날짜 연산

### 시스템 날짜 확인

```sql
SELECT CURRENT_TIMESTAMP;
```

| CURRENT_TIMESTAMP |
|---|
| 2026-07-03 00:17:27 |

```sql
-- 날짜만 확인
SELECT CURRENT_DATE;
```

| DB | 함수 |
|---|---|
| 표준 SQL | `CURRENT_TIMESTAMP` |
| Oracle | `SYSDATE` |
| SQL Server | `GETDATE()` |

> 표준 SQL 함수 사용을 권장.

### 날짜 덧셈/뺄셈

```sql
SELECT CURRENT_DATE + INTERVAL 1 DAY;
```

| CURRENT_DATE + INTERVAL 1 DAY |
|---|
| 2026-07-04 |

### 날짜 간 차이 계산

| DB | 문법 |
|---|---|
| MySQL | `DATEDIFF('2014-02-28', '2014-01-01')` |
| Oracle | `'2014-02-28' - '2014-01-01'` |

### 날짜 서식 변환 (Oracle 예시)

```sql
TO_DATE('2014/01/25', 'YYYY/MM/DD')  -- 문자열 → 날짜형
TO_CHAR(날짜데이터, 'YYYY/MM/DD')    -- 날짜형 → 문자열
```

---

## 10. CASE 문

### 검색 CASE

```sql
CASE
  WHEN 조건식1 THEN 식1
  [WHEN 조건식2 THEN 식2 ...]
  [ELSE 식n]
END
```

```sql
SELECT a,
  CASE WHEN a IS NULL THEN 0 ELSE a END "a(NULL=0)"
FROM sample37;
```

| a | a(NULL=0) |
|---|---|
| 1 | 1 |
| 2 | 2 |
| NULL | 0 |

> NULL 변환이 목적이면 `COALESCE` 함수가 더 간결함.

```sql
SELECT a, COALESCE(a, 0) FROM sample37;
```

### 단순 CASE

```sql
CASE 식1
  WHEN 식2 THEN 식3
  [WHEN 식4 THEN 식5 ...]
  [ELSE 식n]
END
```

```sql
-- 검색 CASE
SELECT a AS "코드",
  CASE
    WHEN a = 1 THEN '남자'
    WHEN a = 2 THEN '여자'
    ELSE '미지정'
  END AS "성별"
FROM sample37;

-- 단순 CASE (동일한 결과)
SELECT a AS "코드",
  CASE a
    WHEN 1 THEN '남자'
    WHEN 2 THEN '여자'
    ELSE '미지정'
  END AS "성별"
FROM sample37;
```

| 코드 | 성별 |
|---|---|
| 1 | 남자 |
| 2 | 여자 |
| NULL | 미지정 |

### CASE 문 주의사항

| 주의사항 | 내용 |
|---|---|
| ELSE 생략 | 생략 시 `ELSE NULL`로 처리됨. 명시하는 것을 권장 |
| 단순 CASE에서 NULL 비교 | `WHEN NULL`은 `=` 연산자로 비교하므로 항상 거짓. NULL 판정은 검색 CASE에서 `IS NULL` 사용 |

```sql
-- NULL 판정은 검색 CASE로
CASE
  WHEN a = 1 THEN '남자'
  WHEN a = 2 THEN '여자'
  WHEN a IS NULL THEN '데이터 없음'
  ELSE '미지정'
END
```

### DB별 동등 함수 비교

| 기능 | 표준 SQL | Oracle | SQL Server |
|---|---|---|---|
| 디코드 | `CASE` 문 | `DECODE` | - |
| NULL 변환 | `COALESCE` | `NVL` | `ISNULL` |

> 이식성을 위해 표준 SQL 함수(`CASE`, `COALESCE`) 사용 권장.
