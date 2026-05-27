### 3-1-3. id를 이용하여 셀렉터 만들기

`class`와 달리 `id` 값은 고유해야 한다. 한 페이지에 같은 `id` 값을 중복 사용하지 않는 것을 권장한다.

```html
<style>
    #target1 {
        font-size: 40px;
        color: blue;
    }
    #target2 {
        font-size: 40px;
        color: red;
    }
</style>
<body>
    <h1>h1 태그</h1>
    <h2 id="target1">h2 태그</h2>
    <a href="/" id="target2">a 태그</a>
</body>
```

- `id`를 표현할 때는 샵(`#`)을 붙인다.
- `id` 값은 해당 페이지에서 고유하게 접근할 수 있는 속성이다.
- 웹에서는 권장사항을 지키지 않아도 바로 종료되지는 않지만, 중복 사용은 권장하지 않는다.

---

### 3-1-4. 복잡한 셀렉터 만들기

부모 태그와 자식 태그를 나열하여 복잡한 셀렉터를 만들 수 있다. 자식 태그를 표현할 때는 **띄어쓰기**를 사용한다.

```html
<style>
    .title {
        font-size: 20px;
        color: green;
    }
    div#container p {
        font-size: 12px;
        color: #7c7c7c;
    }
    div#container div#wrap1 h3 {
        color: red;
    }
    div#container div#wrap2 h3 {
        color: blue;
    }
</style>
<body>
    <div id="container">
        <div id="wrap1">
            <h1 class="title">부모 태그 : wrap1</h1>
            <h3>h3 태그</h3>
        </div>
        <div id="wrap2">
            <h1 class="title">부모 태그 : wrap2</h1>
            <h3>h3 태그</h3>
        </div>
        <p id="comment">저자: 박정태</p>
    </div>
</body>
```

| 셀렉터 | 의미 |
|---|---|
| `.title` | `class="title"`인 모든 요소 |
| `div#container p` | `id="container"`인 `div`의 자식 태그 중 `p` 태그 |
| `div#container div#wrap1 h3` | `id="container"` → `id="wrap1"` → `h3` 순서로 찾아 내려감 |

> 원하는 곳에 접근하는 셀렉터를 자유자재로 만들 수 있어야 크롤러를 만드는 데 어려움이 없다.

---

## 4. JavaScript

JavaScript를 이용하면 웹 사이트에 기능을 추가할 수 있다. `script` 태그를 이용하여 작성하며, `head`보다 `body` 하단에 넣는 것을 권장한다.

```html
<head>
    <script>
        alert('경고창')
    </script>
</head>
```

JavaScript에서 가장 중요한 점은 **HTML 코드를 동적으로 생성할 수 있다**는 것이다. 이렇게 생성된 HTML 코드는 크롤러가 접근하기 힘들다.

```html
<body>
    <div id="container"></div>

    <script>
        document.getElementById('container').innerHTML = "<h1>박정태</h1>";
    </script>
</body>
```

> 웹 사이트에서 HTML로 기본 레이아웃만 잡고, JavaScript로 서버에서 데이터를 가져와 화면에 표시하는 경우가 있다. 크롤러를 만들면서 분석하기 가장 힘든 부분이다.

---

## 5. 웹 렌더링의 원리

크롤러를 만들기 위해 웹 렌더링의 원리를 정확히 이해해야 한다.

| 확인 방법 | 결과 |
|---|---|
| 소스 보기 | 실제 HTML 원본 코드 |
| 개발자 도구 (Elements) | JavaScript 실행 후 DOM으로 표현된 결과 |

**DOM(Document Object Model)** 이란 HTML을 시각적으로 쉽게 표현하기 위해 만든 객체로, 문서를 구조화한 것이다. 개발자 도구에서 태그 옆 삼각형을 누르면 자식 태그를 펼쳐볼 수 있는데, 이 구조를 DOM 구조라고 한다.

크롤러는 JavaScript가 실행되기 전의 원본 HTML 코드를 가져온다. 그렇기 때문에 개발자 도구로 웹을 분석하더라도, **소스 보기에서 해당 요소가 실제로 존재하는지 반드시 확인**해야 한다.

수집하려는 요소가 소스 보기에 없다면 다음 순서로 확인한다.

1. **[Network] 탭** → 서버에서 데이터를 받아오는지 확인
2. 위 방법으로도 해결이 안 된다면 **Selenium** 라이브러리 사용
