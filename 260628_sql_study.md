# MySQL 기초 - ORDER BY / LIMIT

**2026.06.28**  
`MySQL` `SQL` `ORDER BY` `LIMIT` `OFFSET` `정렬`

---

## 1. ORDER BY - 정렬

```sql
-- 기본 구문
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명;

-- WHERE 생략 시
SELECT 열명 FROM 테이블명 ORDER BY 열명;
```

구 순서: `SELECT` → `FROM` → `WHERE` → `ORDER BY`

ORDER BY는 결과를 정렬해서 반환할 뿐, 테이블에 저장된 데이터 자체는 변경하지 않음.

### 기본 정렬 (오름차순 ASC)

```sql
SELECT * FROM sample31 ORDER BY age;
```

| name | age | address |
|---|---|---|
| B씨 | 18 | 부산광역시 연제구 |
| C씨 | 25 | 서울특별시 중구 |
| A씨 | 36 | 대구광역시 중구 |

### 내림차순 DESC

```sql
SELECT * FROM sample31 ORDER BY age DESC;
```

| name | age | address |
|---|---|---|
| A씨 | 36 | 대구광역시 중구 |
| C씨 | 25 | 서울특별시 중구 |
| B씨 | 18 | 부산광역시 연제구 |

### 정렬 방법 비교

| 키워드 | 방향 | 생략 가능 여부 |
|---|---|---|
| `ASC` | 오름차순 | 생략 가능 (기본값) |
| `DESC` | 내림차순 | 생략 불가 |

---

## 2. 대소관계 - 문자열 vs 수치형

문자열형은 사전식 순서로 비교하므로 수치 정렬과 결과가 다름.

```sql
-- a열: VARCHAR(문자열형) → 사전식 정렬
SELECT * FROM sample311 ORDER BY a;
```

| a | b |
|---|---|
| 1 | 1 |
| 10 | 10 |
| 11 | 11 |
| 2 | 2 |

```sql
-- b열: INTEGER(수치형) → 수치 정렬
SELECT * FROM sample311 ORDER BY b;
```

| a | b |
|---|---|
| 1 | 1 |
| 2 | 2 |
| 10 | 10 |
| 11 | 11 |

> 열의 자료형에 따라 정렬 기준이 달라지므로 주의.

---

## 3. 복수 열 정렬

콤마(`,`)로 열을 구분해 지정. 앞에 지정한 열을 먼저 정렬하고, 같은 값이 있을 경우 다음 열 기준으로 정렬.

```sql
SELECT * FROM sample32 ORDER BY a, b;
```

| a | b |
|---|---|
| 1 | 1 |
| 1 | 2 |
| 1 | 3 |
| 2 | 1 |
| 2 | 2 |

각 열에 개별적으로 정렬 방향 지정 가능.

```sql
SELECT * FROM sample32 ORDER BY a ASC, b DESC;
```

| a | b |
|---|---|
| 1 | 3 |
| 1 | 2 |
| 1 | 1 |
| 2 | 2 |
| 2 | 1 |

---

## 4. NULL 값의 정렬 순서

표준 SQL에 규정 없음. DB 제품마다 다름.

| DB | NULL 취급 |
|---|---|
| MySQL | 가장 작은 값으로 취급 → ASC: 맨 앞, DESC: 맨 뒤 |

---

## 5. LIMIT - 행수 제한

표준 SQL 아님. MySQL, PostgreSQL에서 사용 가능.  
ORDER BY 뒤, 구문의 가장 마지막에 지정.

```sql
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명 LIMIT 행수;
```

```sql
SELECT * FROM sample33 LIMIT 3;
```

| no |
|---|
| 1 |
| 2 |
| 3 |

```sql
-- 내림차순 정렬 후 상위 3건
SELECT * FROM sample33 ORDER BY no DESC LIMIT 3;
```

| no |
|---|
| 7 |
| 6 |
| 5 |

> LIMIT은 WHERE로 검색 → ORDER BY 정렬 후 최종적으로 처리됨. WHERE 조건과 내부 처리 순서가 다름.

### DB별 행 제한 문법 비교

| DB | 문법 |
|---|---|
| MySQL / PostgreSQL | `LIMIT 3` |
| SQL Server | `SELECT TOP 3 * FROM 테이블명` |
| Oracle | `WHERE ROWNUM <= 3` |

> Oracle의 ROWNUM은 WHERE 구로 처리되므로 정렬 전에 행이 제한됨. LIMIT과 결과가 다를 수 있음.

---

## 6. OFFSET - 시작 위치 지정

```sql
SELECT 열명 FROM 테이블명 LIMIT 행수 OFFSET 시작행;
```

OFFSET 기본값은 0 (생략 가능).

```sql
-- 1페이지: 1~3행
SELECT * FROM sample33 LIMIT 3 OFFSET 0;
```

| no |
|---|
| 1 |
| 2 |
| 3 |

```sql
-- 2페이지: 4~6행
SELECT * FROM sample33 LIMIT 3 OFFSET 3;
```

| no |
|---|
| 4 |
| 5 |
| 6 |
