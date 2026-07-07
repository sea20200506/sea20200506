# MySQL 기초 - 인덱스 / 뷰 / 집합 연산 / 테이블 결합 / 관계형 모델

**2026.07.07**  
`MySQL` `SQL` `인덱스` `뷰` `UNION` `JOIN` `관계형모델`

---

## 1. 인덱스

테이블에 붙여진 색인. 검색속도 향상이 목적.  
인덱스는 테이블과 독립된 DB 객체로, 테이블 삭제 시 함께 삭제됨.

### 검색 알고리즘 비교

| 방법 | 설명 | 특징 |
|---|---|---|
| 풀 테이블 스캔 | 처음부터 모든 값을 순서대로 조사 | 행 수에 비례해 비교횟수 증가 |
| 이진 탐색 | 집합을 반으로 나누어 검색 | 데이터 수가 2배 되어도 비교횟수 1회만 증가 |
| 이진 트리 | 이진 탐색에 최적화된 데이터 구조 | 노드의 왼쪽=작은 값, 오른쪽=큰 값. 중복값 등록 불가 |

> 이진 트리에서 중복값을 허용하지 않는 특성 때문에 기본키 제약에 이진 트리 인덱스를 사용하는 DB가 많음.

---

## 2. 인덱스 작성과 삭제

표준 SQL에는 없는 명령. 대부분의 DB 제품에서 지원.

### 인덱스 작성

```sql
CREATE INDEX 인덱스명 ON 테이블명 (열명1, 열명2, ...);

-- 예시
CREATE INDEX isample65 ON sample62(a);
```

- 열은 복수 지정 가능
- 행이 대량이면 작성 시간과 저장공간 많이 소비
- INSERT 시 인덱스 갱신 처리가 추가되어 속도 저하

### 인덱스 삭제

```sql
-- 스키마 객체인 경우 (Oracle, DB2)
DROP INDEX 인덱스명;

-- 테이블 내 객체인 경우 (MySQL, SQL Server)
DROP INDEX 인덱스명 ON 테이블명;
```

### 인덱스 네임스페이스

| DB | 인덱스 위치 |
|---|---|
| Oracle, DB2 | 스키마 객체 → 스키마 내 이름 중복 불가 |
| MySQL, SQL Server | 테이블 내 객체 → 테이블 내 이름 중복 불가 |

---

## 3. EXPLAIN - 실행계획 확인

인덱스 사용 여부를 확인하는 명령. 실제 SQL은 실행되지 않음.

```sql
EXPLAIN SELECT * FROM sample62 WHERE a = 'a';
```

- `possible_keys`: 사용 가능한 인덱스
- `key`: 실제 사용된 인덱스
- 인덱스 미사용 시 두 항목 모두 NULL

> DB 내부의 최적화 처리가 인덱스 사용 여부를 판단.  
> '예/아니오' 같이 값의 종류가 적은 열은 인덱스를 만들어도 효율이 낮음.

---

## 4. 뷰

FROM 구의 서브쿼리에 이름을 붙여 DB 객체화한 것. **가상 테이블**.  
저장되는 것은 SELECT 명령뿐 (데이터 저장공간 없음).

```sql
-- 서브쿼리 방식
SELECT * FROM (SELECT * FROM sample54) sq;

-- 뷰 방식
SELECT * FROM sample_view_67;
```

### 뷰 작성

```sql
CREATE VIEW 뷰명 AS SELECT 명령;

-- 예시
CREATE VIEW sample_view_67 AS SELECT * FROM sample54;

-- 열 이름 지정
CREATE VIEW sample_view_672(n, v, v2) AS
  SELECT no, a, a*2 FROM sample54;
```

- `AS` 생략 불가 (별명의 AS와 다름)
- 열 지정 생략 시 SELECT 구의 열이 자동 지정
- 열 이름만 지정 가능 (자료형, 제약 지정 불가)

### 뷰 삭제

```sql
DROP VIEW sample_view_67;
```

### 뷰의 약점과 보완

| 약점 | 보완 방법 |
|---|---|
| 참조할 때마다 SELECT 명령 재실행 → CPU 소비 | 머티리얼라이즈드 뷰 (Oracle, DB2만 지원. MySQL 미지원) |
| 상관 서브쿼리는 뷰의 SELECT 명령으로 사용 불가 | 함수 테이블 (인수로 WHERE 조건 전달 가능) |

> 머티리얼라이즈드 뷰: 결과를 저장장치에 저장. 원본 테이블 변경 시 자동 재실행.  
> SELECT에서만 사용하는 것을 권장. INSERT/UPDATE/DELETE는 조건이 맞을 때만 가능하며 주의 필요.

---

## 5. UNION - 합집합

```sql
SELECT * FROM sample71_a
UNION
SELECT * FROM sample71_b;
```

| a |
|---|
| 1 |
| 2 |
| 3 |
| 10 |
| 11 |

- 세미콜론은 마지막에만
- 각 SELECT의 열 개수와 자료형이 일치해야 함
- 기본 동작은 중복 제거 (DISTINCT)
- 나열 순서는 결과에 영향 없음 (행 순서는 달라질 수 있음)

### ORDER BY

```sql
-- 에러: 중간 SELECT에 ORDER BY 불가
SELECT a FROM sample71_a ORDER BY a
UNION
SELECT b FROM sample71_b;

-- 올바른 방법: 마지막 SELECT에 ORDER BY, 별명으로 통일
SELECT a AS c FROM sample71_a
UNION
SELECT b AS c FROM sample71_b ORDER BY c;
```

### UNION ALL - 중복 포함

```sql
SELECT * FROM sample71_a
UNION ALL
SELECT * FROM sample71_b;
```

| a |
|---|
| 1 |
| 2 |
| 3 |
| 2 |
| 10 |
| 11 |

> 중복값이 없는 경우 UNION ALL이 성능상 유리.  
> `UNION DISTINCT` 문법은 허용되지 않음.

### 교집합 / 차집합

| 연산 | SQL | 비고 |
|---|---|---|
| 교집합 | `INTERSECT` | MySQL 미지원 |
| 차집합 | `EXCEPT` | MySQL 미지원. Oracle은 `MINUS` |

---

## 6. 테이블 결합

### 교차결합 (곱집합)

FROM 구에 복수 테이블을 쉼표로 지정 → 가로 방향으로 결합.

```sql
SELECT * FROM sample72_x, sample72_y;
-- 3 X 3 = 9행 반환
```

> UNION(세로 결합) vs 교차결합(가로 결합)

### 내부결합 (INNER JOIN)

곱집합에서 결합 조건을 만족하는 행만 추출.

```sql
-- 구식 방법 (WHERE에 결합 조건)
SELECT 상품.상품명, 재고수.재고수
FROM 상품, 재고수
WHERE 상품.상품코드 = 재고수.상품코드
AND 상품.상품분류 = '식료품';

-- 권장 방법 (INNER JOIN + ON)
SELECT 상품.상품명, 재고수.재고수
FROM 상품 INNER JOIN 재고수
ON 상품.상품코드 = 재고수.상품코드
WHERE 상품.상품분류 = '식료품';
```

| 상품명 | 재고수 |
|---|---|
| 상품1 | 200 |
| 상품2 | 500 |

### 테이블 별명 활용

```sql
SELECT S.상품명, M.메이커명
FROM 상품2 S INNER JOIN 메이커 M
ON S.메이커코드 = M.메이커코드;
```

### 외부키 (Foreign Key)

다른 테이블의 기본키를 참조하는 열.  
예: 상품 테이블의 `메이커코드` → 메이커 테이블의 기본키 참조.

### 자기결합 (Self Join)

같은 테이블끼리 결합. 반드시 별명으로 구별해야 함.

```sql
SELECT S1.상품명, S2.상품명
FROM 상품 S1 INNER JOIN 상품 S2
ON S1.상품코드 = S2.상품코드;
```

### 외부결합 (LEFT / RIGHT JOIN)

한쪽에만 존재하는 행도 포함해 결합. 일치하는 행이 없으면 NULL.

```sql
SELECT 상품3.상품명, 재고수.재고수
FROM 상품3 LEFT JOIN 재고수
ON 상품3.상품코드 = 재고수.상품코드
WHERE 상품3.상품분류 = '식료품';
```

| 상품명 | 재고수 |
|---|---|
| 상품1 | 200 |
| 상품2 | 500 |
| 추가상품 | NULL |

| 결합 방식 | 설명 |
|---|---|
| `INNER JOIN` | 양쪽 모두 일치하는 행만 반환 |
| `LEFT JOIN` | 왼쪽 테이블 기준, 오른쪽에 없으면 NULL |
| `RIGHT JOIN` | 오른쪽 테이블 기준, 왼쪽에 없으면 NULL |

> 표준 SQL 권장: 내부결합 `INNER JOIN`, 외부결합 `LEFT/RIGHT JOIN`.  
> 구식 쉼표 방식은 MySQL에서 외부결합 불가.

---

## 7. 관계형 모델

### 용어 대응

| 관계형 모델 | SQL |
|---|---|
| 릴레이션 (Relation) | 테이블 |
| 속성 (Attribute) | 열 |
| 튜플 (Tuple) | 행 |

### 관계대수 연산과 SQL 대응

| 관계대수 | SQL |
|---|---|
| 합집합 | `UNION` |
| 차집합 | `EXCEPT` |
| 교집합 | `INTERSECT` |
| 곱집합 | `FROM` 구에 복수 테이블 / `CROSS JOIN` |
| 선택 (튜플 추출) | `WHERE` 구 |
| 투영 (속성 추출) | `SELECT` 구 |
| 결합 | `INNER JOIN` / `OUTER JOIN` |

관계대수의 기본규칙:
- 하나 이상의 관계를 바탕으로 연산
- 연산 결과도 관계
- 연산을 중첩 구조로 실행 가능
