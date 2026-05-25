# Matplotlib 기초 정리

**날짜:** 2026-05-23  
**태그:** `Python` `Matplotlib` `NumPy` `데이터시각화`

---

## 1. 데이터 생성 — `np.arange`

`np.arange(start, stop, step)`으로 균일한 간격의 배열을 생성한다.  
여기서는 0부터 12까지 0.01 간격으로 만든 배열을 `np.sin()`에 입력해 sin 값을 얻는다.

```python
import numpy as np

t = np.arange(0, 12, 0.01)
y = np.sin(t)
```

---

## 2. 기본 그래프 — `plot`

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 6))
plt.plot(t, y)
plt.grid()
plt.xlabel('time')
plt.ylabel('Amplitude')
plt.title('Example of sinewave')
plt.show()
```

| 명령 | 역할 |
|---|---|
| `xlabel` / `ylabel` | x축 / y축 라벨 |
| `title` | 그래프 제목 |
| `grid()` | 격자 표시 |

---

## 3. 다중 그래프 & 범례

`plot`을 여러 번 호출하면 한 화면에 여러 그래프를 그릴 수 있다.  
`label` 옵션과 `plt.legend()`를 함께 써야 범례가 표시된다. `lw` 옵션으로 선 굵기를 지정한다.

```python
plt.figure(figsize=(10, 6))
plt.plot(t, np.sin(t), lw=3, label='sin')
plt.plot(t, np.cos(t), 'r', label='cos')
plt.grid()
plt.legend()
plt.xlabel('time')
plt.ylabel('Amplitude')
plt.title('Example of sinewave')
plt.show()
```

---

## 4. 색상 & 선 스타일

```python
plt.figure(figsize=(10, 6))
plt.plot(t, y, color='green', linestyle='dashed')
plt.show()
```

- `color` : 선 색상 지정
- `linestyle='dashed'` : 점선 스타일

---

## 5. 마커 & 축 범위

```python
plt.figure(figsize=(10, 6))
plt.plot(t, y, color='green', linestyle='dashed', marker='o',
         markerfacecolor='blue', markersize=12)
plt.xlim([-0.5, 6.5])
plt.ylim([0.5, 9.5])
plt.show()
```

- `marker` : 데이터 위치에 마커 표시
- `markerfacecolor` / `markersize` : 마커 색상 및 크기
- `xlim` / `ylim` : x축 / y축 표시 범위 지정

---

## 6. 산점도 — `scatter`

선이 아닌 점으로 데이터를 표현할 때 사용한다.  
`c` 옵션으로 color map을 지정하면 x값에 따라 색상을 다르게 표시할 수 있고, `s` 옵션으로 마커 크기를 지정한다.

```python
t = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
y = np.array([9, 8, 7, 9, 8, 3, 2, 4, 3, 4])

plt.figure(figsize=(10, 6))
plt.scatter(t, y)
plt.show()
```

---

## 7. 정규분포 난수 생성 & 박스플롯

`np.random.normal()`로 정규분포를 따르는 난수를 생성한다.  
같은 데이터를 `plot`과 `boxplot` 두 가지 방식으로 시각화할 수 있다.

```python
s1 = np.random.normal(loc=0, scale=1, size=1000)
s2 = np.random.normal(loc=5, scale=0.5, size=1000)
s3 = np.random.normal(loc=10, scale=2, size=1000)

# 선 그래프
plt.figure(figsize=(10, 6))
plt.plot(s1, label='s1')
plt.plot(s2, label='s2')
plt.plot(s3, label='s3')
plt.legend()
plt.show()

# 박스플롯
plt.figure(figsize=(10, 6))
plt.boxplot((s1, s2, s3))
plt.grid()
plt.show()
```

| 옵션 | 역할 |
|---|---|
| `loc` | 평균값 |
| `scale` | 표준편차 |
| `size` | 생성할 데이터 수 |

> **boxplot**은 데이터의 중앙값, 분포, 이상치를 한눈에 확인할 때 쓴다.

---

*Study log | Python Data Analysis Series*
