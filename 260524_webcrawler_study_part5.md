### 2-2-3. ul, li 태그

리스트를 만드는 태그다. `ul`과 `ol` 태그로 리스트를 만들고, 각 항목은 `li` 태그로 표현한다.

```html
<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
    </ul>

    <ol>
        <li>10</li>
        <li>11</li>
        <li>12</li>
    </ol>
</body>
```

---

### 2-2-4. table 태그

표를 표현하는 태그다. 과거에는 테이블뿐 아니라 페이지 레이아웃을 잡는 역할로도 많이 사용했다.

```html
<body>
    <table>
        <thead>
            <tr>
                <th>1</th>
                <th>2</th>
                <th>3</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>11</td>
                <td>22</td>
                <td>33</td>
            </tr>
        </tbody>
    </table>
</body>
```

| 태그 | 설명 |
|---|---|
| `<table>` | 표 전체를 감싸는 태그 |
| `<thead>` | 표의 헤더 영역 |
| `<tbody>` | 표의 본문 영역 |
| `<tr>` | 행(row)을 표현 |
| `<th>` | 헤더 컬럼. 가운데 정렬 + 굵은 글씨 자동 적용 |
| `<td>` | 일반 컬럼 |

---

### 2-2-5. input, button 태그

데이터를 입력하는 폼과 버튼을 만드는 태그다.

```html
<body>
    <input type="text" value="test1">
    <input type="text" value="test2">
    <input type="text" value="test3">

    <button>bt1</button>
    <button>bt2</button>
    <button>bt3</button>
</body>
```

- `input` 태그는 `type`, `name`, `value` 속성을 가진다.
- `input` 태그에 `type="button"`을 지정하면 버튼으로도 만들 수 있다.
- `input` 태그는 닫는 태그가 존재하지 않는다.

---

### 2-2-6. select 태그

선택 리스트를 만드는 태그다. `option` 태그로 감싼 항목들이 리스트의 아이템으로 표시된다.

```html
<body>
    <select name="address">
        <option value="">주소선택</option>
        <option value="파주">파주</option>
        <option value="혜화">혜화</option>
        <option value="홍대">홍대</option>
        <option value="종로">종로</option>
    </select>
</body>
```

---

### 2-2-7. a 태그

다른 페이지로 이동할 수 있는 링크를 만드는 태그다. `href` 속성에 이동할 링크를 지정한다.

```html
<body>
    <ul>
        <li>
            <a href="http://www.naver.com">네이버로 이동하기</a>
            <a href="http://www.google.com">구글로 이동하기</a>
        </li>
        <li>
            <a href="a">a로 이동 (상대경로)</a>
        </li>
        <li>
            <a href="/a">/a로 이동 (절대경로)</a>
        </li>
    </ul>
</body>
```

링크를 만드는 방법은 세 가지다.

| 방법 | 예시 | 설명 |
|---|---|---|
| 전체 주소 | `http://www.example.com` | 주소 전체를 작성 |
| 상대경로 | `href="a"` | 현재 위치 기준으로 경로 지정. 현재 페이지에 따라 이동 URL이 달라짐 |
| 절대경로(루트경로) | `href="/a"` | 어느 페이지에서 클릭하든 항상 같은 URL로 이동. 슬래시(`/`)로 시작 |

> 크롤러를 만들 때 가장 관심있게 보는 태그 중 하나가 `a` 태그다. `href` 속성값을 가져와 페이지를 수집한다.

---

### 2-2-8. img 태그

이미지를 표시하는 태그다.

```html
<body>
    <img src="frog.png" alt="개구리">
</body>
```

- `src` : 이미지 경로
- `alt` : 이미지가 정상적으로 불러와지지 않을 경우 대체 텍스트
- `img` 태그의 `src` 값을 가져와 이미지를 다운받는 코드 작성도 가능하다.

---

### 2-2-9. span 태그

글을 넣을 수 있는 태그다. `p` 태그와 달리 자동 줄바꿈 없이 옆으로 나열된다.

```html
<body>
    <span>span1</span>
    <span>span2</span>
    <span>span3</span>
</body>
```

- `p` 태그: `display: block` → 태그 간 자동 줄바꿈
- `span` 태그: `display: inline` → 옆으로 나열
- `br` 태그를 사용하면 `span`을 `p`처럼 줄바꿈하여 사용할 수 있다. `br` 태그는 닫는 태그가 없다.

---

### 2-2-10. div 태그

가장 많이 사용되는 태그로, 화면 레이아웃을 잡는 역할을 한다. 특정 기능은 없고 하위 태그들의 영역을 잡아주는 역할이다.

```html
<body>
    <div>
        <h1>첫 번째 div</h1>
        <span>span1</span>
    </div>

    <div>
        <h1>두 번째 div</h1>
        <p>p1</p>
    </div>

    <div>
        <h1>네 번째 div</h1>
        <div>
            <h2>div 중첩</h2>
            <ul>
                <li>li1</li>
                <li>li3</li>
            </ul>
        </div>
    </div>
</body>
```

태그를 중첩할 경우 밖에 있는 태그를 **상위태그(부모 돔)**, 안에 있는 태그를 **하위태그(자식 돔)**라고 부른다.

> 웹 소스를 보려면 개발자 도구에서 **[Elements]** 탭을 클릭한다. 좌측에 HTML 구조, 우측에 CSS가 나타난다.

> `id`와 `class` 속성을 **셀렉터(selector)**라고 부른다. selector의 존재 유무는 원하는 데이터를 정확하게 수집할 수 있느냐 없느냐의 차이다. 모든 태그에 들어갈 수 있는 속성이며, 크롤러에서 데이터를 파싱하기 위해 반드시 확인해야 한다.

---

## 3. CSS

CSS는 **Cascading Style Sheets**의 약자로 웹 사이트를 꾸며주는 역할을 한다. 크롤러에서 CSS를 알아야 하는 이유는, 특정 태그에 접근하는 방식을 크롤러에서도 똑같이 사용하기 때문이다.

---

## 3-1. 셀렉터 (selector)

CSS로 특정 요소에 접근하는 것을 셀렉터라고 부른다. 셀렉터를 만드는 방법은 두 가지다.

- 태그를 이용하는 방법
- `id`와 `class` 속성을 이용하는 방법

### 3-1-1. 태그를 이용하여 셀렉터 만들기

```html
<style>
    p.p-target {
        font-size: 17px;
        color: blue;
    }

    li.li-target {
        list-style-type: none;
    }
</style>
<body>
    <p>p 태그1</p>
    <p class="p-target">p 태그2</p>

    <ul>
        <li>li1</li>
        <li class="li-target">li3</li>
    </ul>
</body>
```

- `p.p-target` : `p` 태그 중 `class="p-target"`인 요소에만 CSS 적용
- 클래스를 표현할 때 마침표(`.`)를 붙인다.

### 3-1-2. class를 이용하여 셀렉터 만들기

```html
<style>
    .target {
        font-size: 40px;
        color: blue;
    }
</style>
<body>
    <h1>h1 태그</h1>
    <h2 class="target">h2 태그</h2>
    <a href="/" class="target">a 태그</a>
</body>
```

태그 종류에 상관없이 `class="target"`인 요소 모두에 CSS 효과가 적용된다. 즉, 원하는 요소만 수집할 수 있다.

### 3-1-3. id를 이용하여 셀렉터 만들기

`class`와 달리 `id` 값은 고유해야 한다.
