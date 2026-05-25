---

## 웹 (Web)

웹은 월드 와이드 웹(World Wide Web)의 약자로, 인터넷에 연결된 클라이언트들이 정보를 공유할 수 있는 공간이다. 이 공간을 사용할 수 있도록 해주는 것이 웹 브라우저다.

웹에서 보는 페이지 하나하나를 **웹 페이지**, 이러한 페이지가 모인 것을 **웹 사이트**라고 한다. 웹 페이지는 크게 세 가지로 이루어져 있다.

| 구성 요소 | 역할 |
|---|---|
| HTML | 레이아웃 구조 |
| JavaScript | 기능 |
| CSS | 화면 꾸미기 |

---

## 2. HTML

HTML은 **하이퍼텍스트 마크업 언어(Hyper Text Markup Language)**의 약자로, 웹을 이루는 가장 기본이 되는 요소다. `.html` 확장자로 파일을 생성하여 실행하면 웹 페이지를 볼 수 있다.

---

## 2-1. HTML 기본 구조

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
```

꺽쇠(`<>`)로 감싸져 있는 것을 **태그**라고 한다. 태그는 여는 태그(`<>`)와 닫는 태그(`</>`) 한 쌍으로 구성되며, 여는 태그가 있으면 반드시 닫는 태그로 감싸주어야 한다. 태그는 이름과 속성으로 만들 수 있으며, 속성은 여러 개가 될 수 있다.

| 태그 | 설명 |
|---|---|
| `<!DOCTYPE html>` | HTML5 문서임을 명시 |
| `<html lang="en">` | 모든 태그를 감싸는 최상위 태그. `lang` 속성으로 언어 지정 |
| `<head>` | 문서 정보를 담는 태그. `meta`, `title`, `style`, `script`, `link` 태그 포함 가능 |
| `<title>` | 문서의 제목 |
| `<meta>` | 문서의 내용, 키워드, 제작자 등 문서 정보를 담는 태그 |
| `<style>` | CSS 코드가 포함되는 태그 |
| `<link>` | 외부 CSS 파일을 로드하는 태그 |
| `<script>` | JavaScript 코드를 작성하거나 외부 JS 파일을 로드하는 태그. `head` 또는 `body` 하단에 포함 가능 |
| `<body>` | 실제 웹 페이지의 내용을 담는 태그. 크롤링 시 데이터를 파싱하는 핵심 부분 |

**meta 태그 속성 목록**

```html
<meta name="Subject" content="홈페이지 주제">
<meta name="Title" content="홈페이지 이름">
<meta name="Description" content="설명문">
<meta name="Keywords" content="키워드">
<meta name="Author" content="만든 사람 이름">
<meta name="Publisher" content="만든 단체나 회사">
<meta name="Other Agent" content="웹 책임자">
<meta name="Classification" content="카테고리 위치(분류)">
<meta name="Generator" content="생성 프로그램(에디터)">
<meta name="Reply-To(Email)" content="메일 주소">
<meta name="Filename" content="파일 이름">
<meta name="Author-Date(Date)" content="제작일">
<meta name="Location" content="위치">
<meta name="Distribution" content="배포자">
<meta name="Copyright" content="저작권">
<meta name="Robots" content="ALL">
```

---

## 2-2. HTML 태그

### 2-2-1. p 태그

문단을 나타내는 태그다.

```html
<body>
    <p>파이썬 크롤러 책1</p>
    <p>박정태</p>
    <p>정보문화사</p>
</body>
```

### 2-2-2. h 태그

폰트 크기를 설정하는 태그다. 숫자가 작을수록 폰트 크기가 커지며 1~6까지 지원된다.

```html
<body>
    <h1>1</h1>
    <h2>2</h2>
    <h3>3</h3>
    <h4>4</h4>
    <h5>5</h5>
    <h6>6</h6>
</body>
```

### 2-2-3. ul, li 태그

리스트를 만드는 태그다.
