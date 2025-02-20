# SEO

## 1️⃣ 웹 표준, 웹 접근성

### **웹 표준 (Web Standards)**

W3C([World Wide Web Consortium](https://www.w3.org/))에서 권고하는 **웹에서 표준적으로 사용되는 기술이나 규칙**을 말한다.

사용자가 어떠한 운영체제나 브라우저를 사용하더라도 웹페이지가 동일하게 보이고 정상적으로 작동할 수 있도록하는 웹페이지 제작 기법을 담고 있다.

웹 표준은 HTML(구조), CSS(표현), JavaScript (동작)등의 기술을 다룬다.

현재는 *크롬, 엣지, 사파리* 등 최신 웹 브라우저들은 모두 웹 표준을 지원한다.

즉 **웹 표준에 맞추어 웹 페이지를 작성하면 어떤 브라우저든 동일한 결과물을 얻을 수 있다**.

이외에도 웹 표준을 준수하면 **`브라우저 호환성 보장`, `SEO 최적화`, `유지보수 및 확장성 향상`, `웹 접근성 향상` 등의 장점**이 있다.

### 웹 표준 준수 방법

웹 표준을 준수하는 방법은 다음과 같다.

1.  **DOCTYPE 선언**

    웹 페이지 최상단에 DOCTYPE을 선언해 웹 페이지가 어떤 버전의 HTML, XHTML을 사용하는 지 명시한다.

    ```jsx
    <!DOCTYPE html>
    <html lang="ko">
    <head>
        <meta charset="UTF-8">
        <title>HTML !DOCTYPE declaration</title>
    </head>
    <body>
        <p>이 문서는 HTML 문서입니다.</p>
    </body>
    </html>
    ```

2.  **시맨틱 태그 활용 (`<header>`, `<nav>`, `<section>` 등)**
    HTML5에서 추가된 시맨틱 태그를 활용해 웹 페이지 구조를 더욱 명확히 표현할 수 있다.
    https://validator.kldp.org/
3.  **CSS 스타일시트 분리**
    CSS 스타일 시트를 사용해 디자인과 레이아웃을 분리하면 유지 보수 및 확장성을 높일 수 있다.
4.  **웹 접근성 고려 (alt 속성, tabindex 등)**
    모든 사용자가 웹 페이지에 쉽게 접근할 수 있도록 웹 접근성을 고려해야 한다. 예를 들어 시각 장애인이 스크린 리더를 사용해 웹 페이지를 읽을 수 있도록 alt 속성을 활용해 이미지에 대한 설명을 제공하거나, 키보드만으로 모든 기능을 사용할 수 있도록 tabindex 속성을 사용하는 등의 작업이 필요하다.
        ```jsx
        <input type="text" name="name" tabindex="1">
        <input type="text" name="email" tabindex="2">
        <input type="submit" value="Submit" tabindex="3">
        ```

### **웹 접근성 (Web Accessibility)**

웹 접근성이란, 장애 여부, 환경에 관계없이 **모든 사용자가 웹 정보에 접근할 수 있도록 보장하는 것을 말한다.**

웹 접근성을 위해 사용되는 보조 기기에는 자막, 스크린 리더, 자동 완성 기능, 마우스 스틱, 색상 대비 디자인 등이 있다. 하지만 스크린 리더는 스스로 웹페이지의 내용을 분석하지 못하므로 마크업을 작성할 때 접근성 관련 속성을 사용해야 한다.

### 웹 접근성 준수 방법

웹 접근성을 준수하는 방법은 다음과 같다.

1. **시각적 요소**
   시각 장애인은 이미지, 비디오, 그래픽 등을 인식할 수 없기 때문에, 대체 텍스트, 적절한 색상 대비, 텍스트 크기 등을 고려해 시각적 요소를 처리해야 한다.
2. **청각적 요소**
   청각 장애인은 오디오 콘텐츠를 이해할 수 없다. 따라서, 자막, 수화, 오디오 설명 등을 제공하여 청각적 요소를 처리해야 한다.
3. **키보드 접근성 보장**
   지체 장애인은 마우스를 사용하지 못하므로 키보드를 통해 웹 사이트를 이용해야 한다. 따라서, 키보드만으로 웹 사이트를 이용할 수 있도록 구현해야 한다.
4. **WCAG 검사 실시**
   웹 사이트를 구축할 때, WCAG 준수 여부를 검사하여 웹 접근성을 확보할 수 있다.

**WCAG(웹 콘텐츠 접근성 지침) 4대 원칙:** 1. **인식의 용이성**: 대체 텍스트 제공, 다양한 형태로 정보 제공 2. **운용의 용이성**: 키보드 사용 보장, 충분한 사용 시간 제공 3. **이해의 용이성**: 콘텐츠의 이해도와 운용성 보장 4. **내구성**: 다양한 기술 환경에서의 호환성 보장

## 2️⃣ SEO란

SEO는 **'Search Engine Optimization'의 약자로, 검색 엔진 최적화**를 말한다.
이는 웹사이트나 웹페이지를 검색 엔진에서 더 잘 찾을 수 있도록 최적화하는 과정을 말한다.

아무리 웹사이트를 잘 만들었다 해도, SEO 최적화가 되어 있지 않다면 검색엔진 상위에 노출되지 못하고 사용자들이 웹사이트를 찾을 수 없어 무의미할 것이다.

### SEO 최적화를 위한 5가지 방법

### 1. Canonical 태그 설정하기

Canonical 태그는 검색엔진에 어떤 URL이 해당 페이지의 대표 URL인지 알려주는 중요한 역할을 한다

```html
<link rel="canonical" href="https://your-site.com/page" />
```

이는 동일한 콘텐츠가 여러 URL에서 접근 가능할 때 특히 중요하다.

### 2. 메타 태그 최적화

메타 태그는 검색엔진과 소셜 미디어에 페이지 정보를 제공한다.

```html
<head>
  <title>페이지 제목 - 사이트명</title>
  <meta name="description" content="페이지 설명" />
  <meta property="og:title" content="소셜 미디어에서 표시될 제목" />
  <meta property="og:description" content="소셜 미디어 설명" />
</head>
```

### 3. 페이지 성능 최적화

Google은 페이지 로딩 속도를 순위 결정 요소로 사용한다.

다음 요소들을 체크해보자.

- 이미지 최적화 (WebP 형식 사용, 적절한 크기)
- 코드 분할 및 지연 로딩
- 브라우저 캐싱 활용
- CDN 사용

성능 체크는 Google의 PageSpeed Insights를 활용해보자.

### 4. 시맨틱 HTML 구조 사용

검색엔진은 시맨틱 태그를 통해 콘텐츠의 구조를 더 잘 이해할 수 있다.

```html
<header>
  <nav>메뉴 내용</nav>
</header>
<main>
  <article>
    <h1>주요 콘텐츠 제목</h1>
    <section>
      <h2>섹션 제목</h2>
      <p>콘텐츠 내용...</p>
    </section>
  </article>
</main>
<footer>푸터 내용</footer>
```

### 5. sitemap.xml 관리

사이트맵은 검색엔진에 여러분의 웹사이트 구조를 알려주는 중요한 파일이다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    <url>
        <loc>https://your-site.com/</loc>
        <lastmod>2024-02-17</lastmod>
        <changefreq>daily</changefreq>
        <priority>1.0</priority>
    </url>
</urlset>

```

## 3️⃣ **Semantic Tag**

### 시맨틱 태그란?

- 시맨틱 (Semantic)의 사전적 정의는 ‘의미론적’, ‘의미의’ 이다.
- 따라서 시맨틱 태그는 의미를 담은 태그라는 뜻을 가진다.

### 시맨틱 태그의 등장 배경

- HTML 4에서는 HTML 태그가 문서 내용들을 화면에 보여주는 역할만 했기 때문에 화면을 구성하는 역할은 대부분 <div> 태그가 맡았고, 수많은 <div> 태그들은 다시 id 속성으로 구분했다.
- 이때 <div> 태그의 이름은 디자이너나 개발자 개인의 생각에 따라 지정되었는데, 이런 방법은 제작자 자신이 나중에 보더라도 이해하기 어려웠을 뿐만 아니라 담당자가 바뀌면 문서 구조를 이해하는 데 많은 시간이 걸리곤 했다.
- 또한 개발자는 물론이고 시각 장애인 용 화면 낭독기나 검색 엔진에서 소스 코드를 읽을 때도 어느 부분이 메뉴이고 어느 부분이 본문인지 구별할 수가 없었다.
- 따라서 헤더나 메뉴 등 반드시 필요한 요소에 맞는 태그를 미리 약속하여 사용하면 훨씬 이해하기 쉬운 코드가 되기 때문에 **HTML5 표준 안에서 문서 구조를 만드는 시맨틱 태그가 등장**하게 되었다.

### 시맨틱 태그의 장점

- 검색 엔진은 의미론적 마크업을 페이지의 검색 순위에 영향을 줄 수 있는 중요한 키워드로 간주한다. 따라서 검색 엔진 최적화에 유리하다.
- 시각 장애가 있는 사용자가 화면 판독기로 페이지를 탐색할 때 의미론적 마크업을 안내판으로 사용할 수 있다. 즉 웹 접근성 측면에서 이점이 있다.
- 의미없고 클래스 이름이 붙여져있거나 그렇지 않은 끊임없는 div들을 탐색하는 것보다, 의미 있는 코드 블록을 찾는 것이 훨씬 쉽다.
- 유지 보수가 편리하다.

### 시맨틱 태그의 종류

- 시맨틱 태그의 종류는 거의 100여 가지가 있지만 자주 쓰이는 것들 위주로 나열하면 아래와 같다.

![https://yozm.wishket.com/magazine/detail/2495/](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2b6748e-ba74-4d54-a235-94c36f9d61cb/0f58a808-ad6b-45c3-a5f3-6d4881bb12a0/Untitled.png)

https://yozm.wishket.com/magazine/detail/2495/

### <header>

- 헤더 태그는 소개 및 탐색에 도움을 주는 콘텐츠를 나타낸다.
- 제목, 로고, 검색 폼, 작성자 이름 등의 요소를 포함할 수 있다.

### <footer>

- 푸터 태그는 가장 가까운 상위 섹션 콘텐츠 또는 섹션 루트 요소에 대한 바닥글을 나타낸다.
- 푸터에는 일반적으로 해당 섹션의 작성자, 저작권 데이터 또는 관련 문서에 대한 링크에 대한 정보가 포함된다.

### <main>

- 메인 태그는 문서 body의 주요 콘텐츠를 나타낸다.
- 주요 콘텐츠 영역은 문서의 핵심 주제나 앱의 핵심 기능에 직접적으로 연결됐거나 확장하는 콘텐츠로 이루어진다.

### <aside>

- 어사이드 태그는 문서의 주요 내용과 간접적으로만 연관된 부분을 나타낸다.
- 주로 사이드바 혹은 콜아웃 박스로 표현한다.
- 별도로, 따로 둔다는 뜻을 가진다.

![https://yozm.wishket.com/magazine/detail/2495/](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2b6748e-ba74-4d54-a235-94c36f9d61cb/6331c0a4-4f33-4583-a43f-2d1796ab43c0/Untitled.png)

https://yozm.wishket.com/magazine/detail/2495/

### <nav>

- nav 태그는 문서의 주요 탐색 링크 블록을 위한 요소이다. 따라서 문서의 모든 링크가 nav 태그 안에 있을 필요는 없다.
- 대게 footer 태그가 nav 태그에 들어가지 않아도 되는 링크를 포함한다.
- 스크린 리더 등 장애를 가진 사용자를 위한 사용자 에이전트는 최초 렌더링에서 탐색 전용 콘텐츠를 제외할 지 결정할 때 nav 태그를 참고한다.

![https://yozm.wishket.com/magazine/detail/2495/](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2b6748e-ba74-4d54-a235-94c36f9d61cb/0f560279-5a47-4b8c-8821-0817c5f54421/Untitled.png)

https://yozm.wishket.com/magazine/detail/2495/

### <ul>

- ul 태그는 정렬되지 않은 목록을 나타낸다. 보통 불릿으로 표현한다.

### <ol>

- ol 태그는 정렬된 목록을 나타낸다. 보통 숫자 목록으로 표현한다.

### <li>

- li 태그는 목록의 항목을 나타낸다. 반드시 정렬 목록 <ol>이나 비정렬 목록 <ul> 혹은 메뉴 <menu> 안에 위치해야 한다.
- 메뉴와 비정렬 목록에서는 보통 불릿으로 나타내고, 정렬 목록에서는 숫자나 문자를 사용한 오름차순 카운터로 나타낸다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2b6748e-ba74-4d54-a235-94c36f9d61cb/ba351d9e-dc63-4592-a3c5-ff997afb4b2d/Untitled.png)

### <article>

- article 태그는 문서, 페이지, 애플리케이션, 또는 사이트 안에서 독립적으로 구분해 배포하거나 재사용할 수 있는 부분을 나타낸다.
- 게시판과 블로그 글, 매거진이나 뉴스 기사 등에 사용한다.
- 하나의 문서가 여러 개의 <article>을 가질 수 있으며 그 안에는 또 여러 개의 <section>이 존재할 수 있다.

### <section>

- section 태그는 HTML 문서의 독립적인 부분을 나타내며, 더 적합한 의미를 가진 요소가 없을 때 사용한다.
- 보통 section 태그는 제목을 포함하지만, 항상 그런 것은 아니다.

![https://yozm.wishket.com/magazine/detail/2495/](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2b6748e-ba74-4d54-a235-94c36f9d61cb/917c55d4-56a0-49dc-a971-68bd5d2e3d67/Untitled.png)

https://yozm.wishket.com/magazine/detail/2495/

### Headings: <h1> ~ <h6>

- <h1> ~ <h6> 요소는 6단계의 제목을 나타낸다. 각 단계에서 <h1>이 가장 높고 <h6>는 가장 낮다.

![https://yozm.wishket.com/magazine/detail/2495/](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2b6748e-ba74-4d54-a235-94c36f9d61cb/4310a139-4d88-40f3-8c68-8273bdfbf1b6/Untitled.png)

https://yozm.wishket.com/magazine/detail/2495/

### HTML 서식 요소

- HTML에서 서식을 표현할 때는 위와 같은 태그들을 사용하는데,
  왼쪽의 <b>, <i>, <u>,<s>는 단순히 시각적으로 그렇게 보이게 만들 뿐이지만 오른쪽에 있는 <strong>, <em>, <ins>, <del>은 의미적으로 정보를 전달한다.

### <a> / <button>

- <a>와 <button>은 동작 방식의 차이가 있다.
- <a> 태그는 페이지를 이동할 때, <button> 태그는 폼을 제출하거나 행 추가 등 행위(액션)을 할 때 사용한다. 사용하기 전에 이 버튼이 어떻게 쓰일 지 생각해보고 사용한다면 훨씬 시맨틱 한 마크업을 할 수 있다.
