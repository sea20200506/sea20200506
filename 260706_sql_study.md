# MySQL 기초 - 서브쿼리 / DB 객체 / DDL / 제약

**2026.07.06**  
`MySQL` `SQL` `서브쿼리` `EXISTS` `IN` `DDL` `CREATE` `ALTER` `제약` `기본키`

---

## 1. 서브쿼리

SQL 명령문 안에 괄호로 묶어 지정하는 하부 SELECT 명령.  
SELECT / DELETE / UPDATE 의 WHERE 구에서 주로 사용.

```sql
-- DELETE에서 서브쿼리 사용
DELETE FROM sample54 WHERE a = (SELECT MIN(a) FROM sample54);
-- MySQL에서는 동일 테이블 참조 시 에러 → 인라인 뷰로 우회
DELETE FROM sample54 WHERE a = (SELECT a FROM (SELECT MIN(a) AS a FROM sample54) AS x);
```

> MySQL은 데이터 추가/갱신 시 동일 테이블을 서브쿼리에서 직접 참조 불가.  
> 파생 테이블(인라인 뷰)로 한 번 감싸서 우회.

### MySQL 클라이언트 변수 활용

```sql
SET @a = (SELECT MIN(a) FROM sample54);
DELETE FROM sample54 WHERE a = @a;
```

---

## 2. 스칼라 값

SELECT 명령이 반환하는 값의 패턴:

| 패턴 | 설명 |
|---|---|
| 패턴1 | 단일 행, 단일 열 → **스칼라 값** |
| 패턴2 | 복수 행, 단일 열 |
| 패턴3 | 단일 행, 복수 열 |
| 패턴4 | 복수 행, 복수 열 |

스칼라 값 = SELECT 명령이 하나의 값만 반환하는 것.

스칼라 값을 반환하는 서브쿼리 = **스칼라 서브쿼리**.  
스칼라 서브쿼리는 `=` 연산자로 비교 가능.

스칼라 서브쿼리가 되는 조건:
- SELECT 구에서 단일 열 지정
- GROUP BY 없이 집계함수 사용
- WHERE 조건으로 행이 하나만 검색되는 경우

---

## 3. 구별 서브쿼리 사용법

### SELECT 구

```sql
SELECT
  (SELECT COUNT(*) FROM sample51) AS sq1,
  (SELECT COUNT(*) FROM sample54) AS sq2;
```

| sq1 | sq2 |
|---|---|
| 5 | 2 |

- SELECT 구에서는 스칼라 서브쿼리만 사용 가능
- MySQL은 FROM 구 생략 가능. Oracle은 `FROM DUAL` 필요

### SET 구 (UPDATE)

```sql
-- MySQL에서는 동일 테이블 참조 시 에러 → 파생 테이블로 우회 필요
UPDATE sample54 SET a = (SELECT MAX(a) FROM sample54);
```

### FROM 구

```sql
SELECT * FROM (SELECT * FROM sample54) sq;
-- AS 키워드로 별명 지정 가능 (Oracle에서는 AS 생략)
```

- FROM 구에서는 스칼라 서브쿼리가 아니어도 됨
- 중첩(네스티드) 구조로 여러 단계 구성 가능

```sql
-- Oracle에서 정렬 후 상위 N건 추출 (LIMIT 없는 DB에서의 우회)
SELECT * FROM (
  SELECT * FROM sample54 ORDER BY a DESC
) sq
WHERE ROWNUM <= 2;
```

### INSERT + 서브쿼리

```sql
-- VALUES 구에 스칼라 서브쿼리
INSERT INTO sample541 VALUES(
  (SELECT COUNT(*) FROM sample51),
  (SELECT COUNT(*) FROM sample54)
);
```

```sql
-- INSERT SELECT: SELECT 결과 전체를 테이블에 삽입
INSERT INTO sample541 SELECT * FROM sample543;
```

> INSERT SELECT는 데이터 복사/이동 시 자주 사용.

---

## 4. EXISTS

서브쿼리가 반환하는 행이 존재하는지 여부를 판별. 스칼라 값 불필요.

```sql
-- sample552에 일치하는 no가 있으면 '있음'으로 갱신
UPDATE sample551 SET a = '있음'
WHERE EXISTS (SELECT * FROM sample552 WHERE no2 = no);
```

| no | a |
|---|---|
| 1 | NULL |
| 2 | NULL |
| 3 | 있음 |
| 4 | NULL |
| 5 | 있음 |

```sql
-- 존재하지 않으면 '없음'으로 갱신
UPDATE sample551 SET a = '없음'
WHERE NOT EXISTS (SELECT * FROM sample552 WHERE no2 = no);
```

| no | a |
|---|---|
| 1 | 없음 |
| 2 | 없음 |
| 3 | 있음 |
| 4 | 없음 |
| 5 | 있음 |

---

## 5. 상관 서브쿼리

부모 명령과 자식 서브쿼리가 특정 관계를 맺는 구조.

```sql
UPDATE sample551 SET a = '있음'
WHERE EXISTS (SELECT * FROM sample552 WHERE no2 = no);
--                                               ↑ 부모(sample551)의 no 열 참조
```

- 일반 서브쿼리: 단독으로 실행 가능
- 상관 서브쿼리: 부모 명령과 연관되어 있어 단독 실행 불가

열 이름이 겹칠 경우 `테이블명.열명` 형식으로 명확히 지정.

```sql
WHERE sample552.no2 = sample551.no
```

---

## 6. IN

집합 안에 값이 존재하는지 비교. `=` 대신 사용.

```sql
-- 상수 리스트
SELECT * FROM sample551 WHERE no IN (3, 5);

-- 서브쿼리로 집합 지정 (스칼라 서브쿼리 불필요)
SELECT * FROM sample551 WHERE no IN (SELECT no2 FROM sample552);
```

| no | a |
|---|---|
| 3 | 있음 |
| 5 | 있음 |

- `NOT IN`: 집합에 포함되지 않으면 참
- IN의 서브쿼리는 단일 열 반환 필요 (복수 열 불가)

### IN과 NULL 주의사항

- IN은 NULL을 무시하지 않지만 `=` 연산으로 비교하므로 NULL 매칭 불가
- NULL 비교는 `IS NULL` 사용
- **NOT IN에서 집합 안에 NULL이 있으면 결과는 항상 UNKNOWN** (참 반환 안 함)

---

## 7. 데이터베이스 객체

테이블, 뷰, 인덱스 등 DB 내에 정의하는 모든 것.

### 명명 규칙

- 기존 이름, 예약어와 중복 불가
- 숫자로 시작 불가
- `_` 이외의 기호 사용 불가
- 한글은 백쿼트(MySQL) 또는 더블쿼트로 감쌈
- 시스템 허용 길이 초과 불가

### 스키마

객체를 담는 그릇. 스키마가 다르면 이름이 같아도 충돌 없음.

| DB | 스키마 |
|---|---|
| MySQL | `CREATE DATABASE`로 만든 데이터베이스 |
| Oracle | 데이터베이스 + 사용자 계층 구조 |

> 같은 이름 충돌을 방지하는 그릇 구조를 **네임스페이스**라 부름.

---

## 8. DDL - 테이블 작성/삭제/변경

| 명령 | 설명 |
|---|---|
| `CREATE` | 객체 작성 |
| `DROP` | 객체 삭제 |
| `ALTER` | 객체 변경 |

> DML(SELECT, INSERT, DELETE, UPDATE): 데이터 조작  
> DDL(CREATE, DROP, ALTER): 데이터 정의

### CREATE TABLE

```sql
CREATE TABLE sample62 (
  no  INTEGER    NOT NULL,
  a   VARCHAR(30),
  b   DATE
);
```

| Field | Type | Null | Default |
|---|---|---|---|
| no | int | NO | NULL |
| a | varchar(30) | YES | NULL |
| b | date | YES | NULL |

### DROP TABLE

```sql
DROP TABLE 테이블명;
```

- 테이블과 데이터 모두 삭제. 확인 창 없음.
- 데이터만 전체 삭제: `DELETE` (행 단위 처리, 느림)
- 모든 행을 빠르게 삭제: `TRUNCATE TABLE` (WHERE 지정 불가)

### ALTER TABLE

#### 열 추가

```sql
ALTER TABLE sample62 ADD nowcol INTEGER;
```

- 기존 행이 있으면 추가된 열의 값은 모두 NULL
- NOT NULL 열 추가 시 기본값도 반드시 지정해야 함

#### 열 속성 변경

```sql
ALTER TABLE sample62 MODIFY newcol VARCHAR(20);
```

- 열 이름은 변경 불가, 자료형/기본값/NOT NULL 제약 변경 가능
- 기존 데이터가 있으면 변환 처리. 실패 시 명령 전체 롤백

#### 열 이름 변경

```sql
ALTER TABLE sample62 CHANGE newcol c VARCHAR(20);
```

- CHANGE는 이름과 속성을 동시에 변경 가능
- Oracle은 `RENAME TO` 하부명령 사용

#### 열 삭제

```sql
ALTER TABLE sample62 DROP c;
```

#### 최대 길이 변경

```sql
-- 늘리기
ALTER TABLE sample MODIFY col VARCHAR(30);
-- 줄이기: 기존 데이터보다 작게 지정하면 에러. 실제 저장공간이 늘어나는 경우도 드뭄
```

---

## 9. 제약

### 열 제약 vs 테이블 제약

| 구분 | 설명 | 예시 |
|---|---|---|
| 열 제약 | 하나의 열에 설정 | `NOT NULL`, `UNIQUE` |
| 테이블 제약 | 복수 열에 걸쳐 설정 | `PRIMARY KEY (no, sub_no)` |

```sql
-- 열 제약
CREATE TABLE sample631 (
  a INTEGER NOT NULL,
  b INTEGER NOT NULL UNIQUE,
  c VARCHAR(30)
);

-- 테이블 제약 (제약명 지정)
CREATE TABLE sample632 (
  no     INTEGER NOT NULL,
  sub_no INTEGER NOT NULL,
  name   VARCHAR(30),
  CONSTRAINT pkey_sample PRIMARY KEY (no, sub_no)
);
```

### 제약 추가

```sql
-- 열 제약 추가
ALTER TABLE sample631 MODIFY c VARCHAR(30) NOT NULL;

-- 테이블 제약 추가
ALTER TABLE sample631 ADD CONSTRAINT pkey_sample631 PRIMARY KEY(a);
```

> 기존 데이터가 제약을 위반하면 에러 발생.

### 제약 삭제

```sql
-- 열 제약 삭제
ALTER TABLE sample631 MODIFY c VARCHAR(30);

-- 테이블 제약(기본키) 삭제
ALTER TABLE sample631 DROP PRIMARY KEY;
```

---

## 10. 기본키 (PRIMARY KEY)

- 행을 유일하게 특정하는 검색키
- 중복값 불가, NULL 불가
- 테이블당 하나만 설정 가능

```sql
CREATE TABLE sample634 (
  p INTEGER NOT NULL,
  a VARCHAR(30),
  CONSTRAINT pkey_sample634 PRIMARY KEY(p)
);
```

```sql
INSERT INTO sample634 VALUES(1, '첫째줄');  -- OK
INSERT INTO sample634 VALUES(2, '둘째줄');  -- OK
INSERT INTO sample634 VALUES(2, '넷째줄');  -- ERROR: Duplicate entry '2'
```

### 복수 열 기본키

```sql
CREATE TABLE sample632 (
  no     INTEGER NOT NULL,
  sub_no INTEGER NOT NULL,
  name   VARCHAR(30),
  CONSTRAINT pkey_sample PRIMARY KEY (no, sub_no)
);
```

- 키를 구성하는 **모든 열의 조합**이 중복되지 않으면 제약 위반 아님
- a열 값이 같아도 b열이 다르면 허용

| a | b |
|---|---|
| 1 | 1 |
| 1 | 2 |
| 1 | 3 |
| 2 | 1 |
| 2 | 2 |
