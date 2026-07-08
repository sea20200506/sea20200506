# MySQL 기초 - INSERT / DELETE / UPDATE / 집계함수 / GROUP BY

**2026.07.05**  
`MySQL` `SQL` `INSERT` `DELETE` `UPDATE` `COUNT` `GROUP BY` `HAVING` `집계함수`

---

## 1. INSERT - 행 추가

```sql
-- 기본 구문
INSERT INTO 테이블명 VALUES(값1, 값2, 값3);

-- 열 지정
INSERT INTO 테이블명(열1, 열2) VALUES(값1, 값2);
```

```sql
INSERT INTO sample41 VALUES(1, 'ABC', '2014-01-25');
```

| no | a | b |
|---|---|---|
| 1 | ABC | 2014-01-25 |

- INSERT는 실행 결과를 반환하지 않음. SELECT로 별도 확인 필요.
- 열을 지정해 INSERT할 경우, 지정하지 않은 열에는 DEFAULT 값이 저장됨.

```sql
-- a, no 열만 지정 → b열은 NULL(기본값)로 저장
INSERT INTO sample41(a, no) VALUES('XYZ', 2);
```

| no | a | b |
|---|---|---|
| 1 | ABC | 2014-01-25 |
| 2 | XYZ | NULL |

---

## 2. NOT NULL 제약

NULL을 허용하지 않는 열에 NULL을 INSERT하면 에러 발생.

```sql
-- no열에 NOT NULL 제약 → 에러
INSERT INTO sample41(no, a, b) VALUES(NULL, NULL, NULL);

-- no열에 값 지정 → 정상 실행
INSERT INTO sample41(no, a, b) VALUES(3, NULL, NULL);
```

---

## 3. DEFAULT

열을 지정하지 않거나 `DEFAULT` 키워드를 사용하면 테이블 정의 시 설정한 기본값이 저장됨.

```sql
-- 명시적: DEFAULT 키워드 사용
INSERT INTO sample411(no, d) VALUES(2, DEFAULT);

-- 암묵적: 열 자체를 생략
INSERT INTO sample411(no) VALUES(3);
```

| no | d |
|---|---|
| 1 | 1 |
| 2 | 0 |
| 3 | 0 |

> d열의 DEFAULT 값이 0이므로 두 방법 모두 0으로 저장됨.

---

## 4. DELETE - 행 삭제

```sql
DELETE FROM 테이블명 WHERE 조건식;
```

```sql
DELETE FROM sample41 WHERE no = 3;
```

| no | a | b |
|---|---|---|
| 1 | ABC | 2014-01-25 |
| 2 | XYZ | NULL |

- WHERE 생략 시 모든 행 삭제
- 열 단위 삭제 불가 (`DELETE no FROM sample41` 같은 문법은 없음)
- ORDER BY 구 사용 불가
- 실행 전 확인 창 없음 → WHERE 조건 주의

---

## 5. UPDATE - 데이터 갱신

```sql
UPDATE 테이블명 SET 열명 = 값 WHERE 조건식;
```

```sql
UPDATE sample41 SET b = '2014-09-07' WHERE no = 2;
```

| no | a | b |
|---|---|---|
| 1 | ABC | 2014-01-25 |
| 2 | XYZ | 2014-09-07 |

- SET 구의 `=`은 대입 연산자 (비교 연산자 아님)
- WHERE 생략 시 모든 행 갱신
- 존재하지 않는 열 지정 시 에러

### 열 값을 식으로 갱신

```sql
-- 모든 행의 no에 1 더하기
UPDATE sample41 SET no = no + 1;
```

### 복수 열 갱신

```sql
-- 비효율적
UPDATE sample41 SET a = 'xxx' WHERE no = 2;
UPDATE sample41 SET b = '2014-01-01' WHERE no = 2;

-- 권장
UPDATE sample41 SET a = 'xxx', b = '2014-01-01' WHERE no = 2;
```

### SET 구 실행 순서 - DB별 차이

```sql
UPDATE sample41 SET no = no + 1, a = no;
UPDATE sample41 SET a = no, no = no + 1;
```

| DB | 동작 |
|---|---|
| MySQL | SET 구에 기술된 순서대로 처리 → 순서에 따라 결과 다름 |
| Oracle | 갱신식 오른쪽의 열은 항상 갱신 이전 값 참조 → 순서 무관 |

> MySQL에서는 갱신식 안에서 열을 참조할 때 처리 순서를 반드시 고려할 것.

### NULL로 갱신 (NULL 초기화)

```sql
UPDATE sample41 SET b = NULL;
```

> NOT NULL 제약이 설정된 열은 NULL로 갱신 불가.

---

## 6. 물리삭제 vs 논리삭제

| 구분 | 방법 | 특징 |
|---|---|---|
| 물리삭제 | `DELETE` 명령으로 행 직접 삭제 | 저장공간 확보, 복구 불가 |
| 논리삭제 | `삭제플래그` 열을 `UPDATE`로 갱신 | 복구 가능, 저장공간 증가, 검색속도 저하 |

### 선택 기준 예시

| 상황 | 권장 방식 |
|---|---|
| SNS 개인정보 탈퇴 | 물리삭제 (개인정보 유출 방지) |
| 쇼핑몰 주문 취소 | 논리삭제 (통계 등 활용 가능) |
| 저장공간 부족 | 물리삭제 |

---

## 7. 집계함수

복수의 행(집합)으로부터 하나의 값을 계산해 반환.

| 함수 | 설명 |
|---|---|
| `COUNT(집합)` | 행 개수 |
| `SUM(집합)` | 합계 (수치형만 가능) |
| `AVG(집합)` | 평균 (수치형만 가능) |
| `MIN(집합)` | 최솟값 |
| `MAX(집합)` | 최댓값 |

> 집계함수는 NULL을 무시하고 처리. `MIN`, `MAX`는 문자열형, 날짜시간형에도 사용 가능.

---

## 8. COUNT

```sql
SELECT COUNT(*) FROM sample51;
```

| COUNT(*) |
|---|
| 5 |

- `COUNT(*)`: 테이블 전체 행 수 (NULL 포함)
- `COUNT(열명)`: 해당 열에서 NULL을 제외한 행 수
- `*`를 인수로 사용할 수 있는 건 COUNT뿐

```sql
SELECT COUNT(no), COUNT(name) FROM sample51;
```

| COUNT(no) | COUNT(name) |
|---|---|
| 5 | 4 |

> name 열에 NULL이 1개 있으므로 4 반환.

---

## 9. DISTINCT - 중복 제거

```sql
-- 중복 포함 (기본값)
SELECT ALL name FROM sample51;

-- 중복 제거
SELECT DISTINCT name FROM sample51;
```

| name |
|---|
| A |
| B |
| C |
| NULL |

- `ALL` 또는 아무것도 지정하지 않으면 중복 포함
- `DISTINCT`와 `ALL`은 열명이 아닌 예약어

### 집계함수 내에서 DISTINCT

```sql
SELECT COUNT(ALL name), COUNT(DISTINCT name) FROM sample51;
```

| COUNT(ALL name) | COUNT(DISTINCT name) |
|---|---|
| 4 | 3 |

> `COUNT(DISTINCT name)`: NULL 제외 + 중복 제거 후 개수

---

## 10. SUM / AVG / MIN / MAX

```sql
SELECT SUM(quantity) FROM sample51;
-- 결과: 16

SELECT AVG(quantity), SUM(quantity)/COUNT(quantity) FROM sample51;
-- 결과: 4.0000 (두 방법 동일)

SELECT MIN(quantity), MAX(quantity), MIN(name), MAX(name) FROM sample51;
```

| MIN(quantity) | MAX(quantity) | MIN(name) | MAX(name) |
|---|---|---|---|
| 1 | 10 | A | C |

### NULL을 0으로 간주한 평균

```sql
SELECT AVG(CASE WHEN quantity IS NULL THEN 0 ELSE quantity END) AS avgnull0
FROM sample51;
```

| avgnull0 |
|---|
| 3.2000 |

---

## 11. GROUP BY

```sql
SELECT 열명, 집계함수 FROM 테이블명 GROUP BY 열명;
```

```sql
SELECT name, COUNT(name), SUM(quantity) FROM sample51 GROUP BY name;
```

| name | COUNT(name) | SUM(quantity) |
|---|---|---|
| A | 2 | 3 |
| B | 1 | 10 |
| C | 1 | 3 |
| NULL | 0 | NULL |

- GROUP BY 단독 사용은 DISTINCT와 유사한 중복 제거 효과
- **GROUP BY에 지정하지 않은 열은 집계함수 없이 SELECT 구에 쓸 수 없음**

```sql
-- 에러: quantity는 GROUP BY에 없고 집계함수도 아님
SELECT no, name, quantity FROM sample51 GROUP BY name;

-- 올바른 사용
SELECT MIN(no), name, SUM(quantity) FROM sample51 GROUP BY name;
```

### 복수 열 그룹화

```sql
SELECT name, quantity FROM sample51 GROUP BY name, quantity;
```

---

## 12. HAVING - 그룹 조건 지정

집계함수는 WHERE 구에서 사용 불가 → HAVING 구 사용.

```sql
-- 에러
SELECT name, COUNT(name) FROM sample51 WHERE COUNT(name) = 1 GROUP BY name;

-- 올바른 사용
SELECT name, COUNT(name) FROM sample51 GROUP BY name HAVING COUNT(name) = 1;
```

| name | COUNT(name) |
|---|---|
| B | 1 |
| C | 1 |

### 내부 처리 순서

```
WHERE 구 → GROUP BY 구 → HAVING 구 → SELECT 구 → ORDER BY 구
```

> HAVING 구는 SELECT 구보다 앞서 처리되므로 SELECT에서 붙인 별명 사용 불가.  
> ORDER BY 구는 SELECT 구보다 나중에 처리되므로 집계함수 및 별명 사용 가능.

### 그룹화 후 정렬

```sql
SELECT name, COUNT(name), SUM(quantity)
FROM sample51
GROUP BY name
ORDER BY SUM(quantity) DESC;
```

| name | COUNT(name) | SUM(quantity) |
|---|---|---|
| B | 1 | 10 |
| A | 2 | 3 |
| C | 1 | 3 |
| NULL | 0 | NULL |
