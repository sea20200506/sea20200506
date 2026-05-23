
# 파이썬으로 데이터 주무르기

**날짜:** 2026-05-22  
**태그:** `Python` `Pandas` `NumPy` `데이터분석`

---

## 1. Series

pandas의 데이터 유형 중 기초가 되는 것이 Series다.  
파이썬의 list 데이터로 만들 수 있다.

```python
import pandas as pd

s = pd.Series([1, 2, 3, 4])
```

---

## 2. date_range

pandas에는 날짜형 데이터인 date_range가 있다.  
기본 날짜를 지정하고 `periods` 옵션으로 기간을 지정한다.

```python
dates = pd.date_range('20130101', periods=6)
```

---

## 3. DataFrame 생성

6행 4열의 random 변수를 만들고, 컬럼과 인덱스를 지정한다.

```python
import numpy as np

df = pd.DataFrame(
    np.random.randn(6, 4),
    index=dates,
    columns=['A', 'B', 'C', 'D']
)
```

---

## 4. sort_values — 정렬

`by`로 지정한 컬럼을 기준으로 정렬한다.

```python
df.sort_values(by='B', ascending=False)
```

| 옵션 | 역할 |
|---|---|
| `ascending=False` | 내림차순 |
| `ascending=True` | 오름차순 |

---

## 5. iloc — 위치 기반 인덱싱

행이나 열의 위치를 번호로 지정해서 데이터를 가져온다.

```python
df.iloc[[1, 2, 4], [0, 2]]
```

> **loc vs iloc:** `loc`는 라벨(이름) 기반, `iloc`는 번호(위치) 기반이다.

---

## 6. 조건부 필터링

컬럼을 지정할 때 `df['A']`와 `df.A` 두 가지 방식을 사용할 수 있다.  
조건을 만족하지 않는 값은 NaN 처리된다.

```python
df[df.A > 0]    # 컬럼 A가 0보다 큰 행만 반환
df[df > 0]      # 전체에서 조건 적용, 불만족 시 NaN 처리
```

---

## 7. apply — 함수 적용

`apply`를 사용하면 DataFrame의 각 열(또는 행)에 특정 함수를 적용할 수 있다.

```python
df.apply(np.cumsum)                        # 누적합
df.apply(lambda x: x.max() - x.min())     # 최대 - 최소 (범위)
```

---

## 8. drop — 행/열 삭제

```python
pop_Seoul.drop([0], inplace=True)    # 0번 인덱스 행 삭제
del df['column_name']                # 열 삭제
```

- 행 삭제: `drop`
- 열 삭제: `del`

---

## 9. concat — 데이터 병합

`pd.concat`으로 두 DataFrame을 병합한다.

```python
# 기본 병합 (행 방향)
result = pd.concat([df1, df2, df3])

# 열 방향 병합 + inner join (공통 인덱스만 유지)
result = pd.concat([df1, df4], axis=1, join='inner')

# reindex로 df1 인덱스 기준 재정렬
result = pd.concat([df1, df4], axis=1).reindex(df1.index)

# ignore_index=True: 기존 인덱스 무시하고 새로 부여
result = pd.concat([df1, df2], axis=1, ignore_index=True)
```

---

## 10. merge — 키 기반 병합

공통된 key를 기준으로 두 DataFrame을 합친다.

```python
pd.merge(left, right, on='key')                  # 공통 key 기준
pd.merge(left, right, how='left', on='key')      # left 기준
pd.merge(left, right, how='right', on='key')     # right 기준
pd.merge(left, right, how='outer', on='key')     # 합집합 (없는 값은 NaN)
pd.merge(left, right, how='inner', on='key')     # 교집합 (공통 요소만)
```

---

## 11. 상관계수 — `corrcoef`

두 변수 간의 선형 관계 강도를 -1 ~ 1 사이 값으로 나타낸다.  
결과는 행렬로 반환된다.

```python
np.corrcoef(data['컬럼1'], data['컬럼2'])
```

| 범위 | 해석 |
|---|---|
| 0.1 이하 | 거의 무시 |
| 0.3 이하 | 약한 상관관계 |
| 0.7 이하 | 뚜렷한 상관관계 |

---

## 12. Matplotlib 시각화

파이썬의 대표 시각화 라이브러리는 Matplotlib이며, 그래프를 그리는 모듈은 `matplotlib.pyplot`이다.

```python
import matplotlib.pyplot as plt

%matplotlib inline   # Jupyter에서 그래프를 출력 셀에 바로 표시

plt.plot(df)
plt.show()
```

---

*Study log | Python Data Analysis Series*
