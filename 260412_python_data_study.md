# 파이썬으로 데이터 주무르기

**날짜:** 2026-04-12  
**태그:** `Python` `Pandas` `NumPy` `데이터분석`

---

## 1. 파일 경로 — 윈도우 역슬래시 처리

윈도우 경로는 역슬래시(`\`)를 **두 번** 써야 오류가 나지 않는다.  
단일 역슬래시는 파이썬에서 이스케이프 문자로 인식된다.

```python
# 오류 발생
path = "C:\Users\data\file.csv"

# 올바른 방법 (이중 역슬래시)
path = "C:\\Users\\data\\file.csv"

# 또는 raw string 사용
path = r"C:\Users\data\file.csv"
```

---

## 2. 열(Column) 확인 및 이름 변경

```python
# 열 이름 확인
df.columns           # 모든 열 이름 반환
df.columns[0]        # 첫 번째 열 이름 (예: '기관명')

# 단일 열 이름 변경
df.rename(columns={df.columns[0]: '바꾼이름'}, inplace=True)

# 여러 열 이름 한 번에 변경
df.rename(columns={
    df.columns[0]: '바꾼이름',
    df.columns[1]: '변경이름'
}, inplace=True)
```

> **inplace=True**란? 원본 변수를 직접 수정하라는 옵션.  
> 없으면 변경된 결과가 반환만 되고 원본은 그대로다.

---

## 3. 파일 읽기 — read_excel vs read_csv

> ⚠️ `pd.read_excel()`에는 `encoding` 옵션이 없다. `encoding='utf-8'`을 넣으면 오류 발생!  
> 인코딩 옵션은 `pd.read_csv()`에서만 사용한다.

```python
df = pd.read_excel("파일.xlsx")                    # encoding 옵션 없음
df = pd.read_csv("파일.csv", encoding='utf-8')     # csv는 encoding 가능
```

---

## 4. DataFrame 기본 생성

```python
import pandas as pd
import numpy as np

# 날짜형 인덱스 생성 (20130101부터 6일)
dates = pd.date_range('20130101', periods=6)

# 6행 4열 랜덤 DataFrame 생성
df = pd.DataFrame(
    np.random.randn(6, 4),
    index=dates,
    columns=['A', 'B', 'C', 'D']
)

# pd.Series: 가장 기초가 되는 데이터 유형
s = pd.Series([1, 2, 3, np.nan, 5])   # np.nan = 결측치(NaN)
```

---

## 5. DataFrame 기본 확인 명령어

```python
df.head()       # 상위 5행 확인
df.index        # 인덱스 확인
df.columns      # 열 이름 확인
df.values       # 데이터 값 확인
```

> ⚠️ **흔한 오타:** `df.heads()` → `df.head()`가 맞다. `s`를 붙이면 AttributeError!

---

## 6. 데이터 정렬 및 선택

```python
# 정렬: B 컬럼 기준 내림차순
df.sort_values(by='B', ascending=False)

# 슬라이싱: 0~2행 (3 미포함)
df[0:3]

# loc: 라벨(이름) 기반 슬라이싱
df.loc[:, ['A', 'B']]

# iloc: 번호(위치) 기반 슬라이싱 (끝 번호 미포함)
df.iloc[3:5, 0:2]          # 3~4행, 0~1열
df.iloc[[1, 3], [0, 2]]    # 콤마로 개별 지정도 가능
```

---

## 7. 조건 필터링 및 열 추가

```python
# 열 접근: 두 방법 모두 동일
df['A']
df.A

# 조건 필터: A > 0인 행만 반환, 나머지는 NaN
df[df.A > 0]

# isin: 특정 값 포함 여부 확인
df2['E'].isin(['two', 'four'])
```

---

## 8. apply와 lambda — 함수 적용

```python
# apply: DataFrame에 함수 적용
df.apply(np.cumsum)   # 누적합

# lambda: 최대값 - 최소값 (범위) 계산
df.apply(lambda x: x.max() - x.min())
```

> **apply + lambda**는 각 열(또는 행)에 원하는 함수를 한 줄로 적용할 때 쓴다.  
> 최대·최소 차이, 정규화 등에 자주 활용.

---

*Study log | Python Data Analysis Series*
