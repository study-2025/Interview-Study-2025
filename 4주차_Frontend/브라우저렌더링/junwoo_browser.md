# 브라우저렌더링

## 📌 Notion 문서 링크
[📖 Notion 문서 바로가기](https://bokjunwoo.notion.site/19577d2ca3638037bbdce02202aa190a?pvs=74)

## 🔎 미리보기
## 주소창에 주소를 입력했을때 무슨일이 벌어질까?

### **1. URL 입력 및 처리**

- 사용자가 **브라우저 주소창**에 `https://mapleburningfield.com`을 입력하고 Enter를 누르면, 브라우저는 입력된 주소를 분석합니다.

### **2. DNS 조회 (Domain Name System)**

- 브라우저는 `mapleburningfield.com`에 해당하는 **IP 주소**를 찾기 위해 **DNS 서버**에 질의합니다.
- DNS 조회 과정
    1. **브라우저 캐시** 확인 (이전에 방문한 사이트라면 DNS 정보가 저장되어 있을 수 있음)
    2. **운영체제(OS) 캐시** 확인
    3. **로컬 DNS 서버 (ISP 제공)** 확인
    4. **최종적으로 루트 네임서버를 거쳐 도메인 네임을 IP로 변환**
- 결과적으로 브라우저는 `mapleburningfield.com`이 `172.67.213.64` 같은 IP 주소와 매핑된다는 정보를 얻습니다.

### **3. TCP 연결 (3-Way Handshake)**

- 브라우저는 해당 IP 주소로 **TCP 연결**을 시도합니다.
- TCP 3-Way Handshake 과정:
    1. **SYN**: 클라이언트(브라우저)가 서버에 연결 요청을 보냄.
    2. **SYN-ACK**: 서버가 요청을 받아들이고 응답.
    3. **ACK**: 클라이언트가 최종 확인을 보내고 연결 완료.
- 연결이 완료되면 이후 데이터를 송수신할 준비가 됩니다.

### **4. HTTPS (SSL/TLS Handshake)**

- 주소창에 `https://`가 포함되어 있다면 **SSL/TLS 암호화** 과정이 진행됩니다.

### **5. HTTP 요청 전송**

- 이제 브라우저는 서버로 HTTP 요청을 보냅니다.

### **6. 서버에서 응답 (HTTP Response)**

- 서버는 요청을 처리하고, 응답을 반환합니다.

### **7. 브라우저가 HTML을 파싱하고 렌더링**

- 브라우저는 받은 **HTML 파일**을 분석하고, 내부에 있는 **CSS, JavaScript, 이미지** 등을 추가로 요청합니다.
- **렌더링 과정**:
    1. HTML 파싱 → **DOM** 생성
    2. CSS 파싱 → **CSSOM** 생성
    3. **Render Tree** 생성
    4. **레이아웃 (Layout)** 계산
    5. **페인팅 (Painting)** 및 최종 화면 출력

### **8. 추가적인 요청 처리**

- HTML 내부에 `<script>`, `<img>`, `<link>` 같은 태그가 포함되어 있으면 추가 요청이 발생합니다.

### **9. 페이지가 사용자에게 표시됨**

- 모든 리소스가 로드되면 브라우저는 최종적으로 웹페이지를 완전히 렌더링하여 사용자에게 표시합니다.
- 이후 **자바스크립트 실행**, **사용자 인터랙션 처리**, **비동기 요청** 등이 발생할 수 있습니다.

## 브라우저는 렌더링을 어떻게 할까?

### 1. **Navigation (네트워크 요청 및 응답)**

- 사용자가 URL을 입력하거나, 링크를 클릭하면 브라우저는 **HTTP 요청**을 보냅니다.
- 서버는 HTML 문서를 응답으로 반환하고, 브라우저는 이를 받아서 처리합니다.

### 2. **HTML Parsing & DOM 생성**

- 브라우저는 받은 HTML 문서를 **파싱(Parsing)** 하여 **DOM (Document Object Model)** 트리를 생성합니다.
- DOM은 HTML 태그를 계층적인 트리 구조로 표현한 것입니다.
    
    <aside>
    💡
    
    DOM 트리는 문서의 내용을 설명합니다. html 요소는 시작하는 태그이고 DOM 트리의 루트 노드입니다. 트리는 다른 태그간의 관계와 계층을 반영합니다. 다른 태그에 감싸져 있는 태그는 자식 노드입니다. DOM 노드의 개수가 많아질수록, DOM 트리를 만드는데 더 오랜 시간이 걸립니다.
    
    예제 HTML
    
    ```html
    <html>
      <head>
        <title>DOM Example</title>
      </head>
      <body>
        <h1>Hello, DOM!</h1>
        <p>This is a paragraph.</p>
      </body>
    </html>
    ```
    
    위 HTML의 DOM 트리
    
    - **Document**: HTML 문서 전체를 의미하는 최상위 객체.
    - **Element Nodes** (`<html>`, `<head>`, `<body>` 등): HTML 태그를 나타냄.
    - **Text Nodes**: 요소 내부의 텍스트를 포함.
    
    ```html
    📂 Document
     ├── 📂 <html>
     │   ├── 📂 <head>
     │   │   ├── 📄 <title> → "DOM Example"
     │   ├── 📂 <body>
     │   │   ├── 📄 <h1> → "Hello, DOM!"
     │   │   ├── 📄 <p> → "This is a paragraph."
    ```
    
    </aside>
    

### 3. **CSS Parsing & CSSOM 생성**

- HTML 문서에서 `<link>` 태그나 `<style>` 태그를 통해 CSS를 로드합니다.
- CSS를 파싱하여 **CSSOM (CSS Object Model)** 트리를 생성합니다.

### 4. **Render Tree 생성**

- DOM과 CSSOM을 결합하여 **Render Tree**를 생성합니다.
- Render Tree는 실제로 화면에 표시해야 할 요소들만 포함하며, `display: none;` 같은 요소는 포함되지 않습니다.

### 5. **Layout (Reflow) - 요소 위치와 크기 계산**

- Render Tree를 기반으로 각 요소의 크기(width, height)와 위치(position)를 계산하는 과정입니다.
- 이 과정에서 부모 요소의 크기에 따라 자식 요소의 크기가 결정되기도 합니다.

### 6. **Painting - 픽셀로 변환**

- Layout 단계에서 계산된 요소들의 위치와 스타일을 바탕으로 실제 화면에 그리는 과정입니다.
- 브라우저는 이 데이터를 **레이어(Layer)** 로 관리하며, 필요하면 GPU 가속을 활용하여 성능을 최적화합니다.

### 7. **Compositing - 화면에 최종 출력**

- 여러 개의 레이어를 조합하여 최종적으로 화면에 렌더링합니다.
- 레이어가 많으면 GPU가 이를 합성(Compositing)하여 빠르게 표시할 수 있습니다.

<aside>
💡

간단한 예

- **DOM 생성:** `<div>Hello</div>` → 브라우저가 `div` 요소를 **DOM Tree**에 추가.
- **CSS 적용:** `div { color: red; }` → 브라우저가 **CSSOM**을 생성.
- **Render Tree 생성:** `div`가 화면에 표시될지 결정.
- **Layout:** `div`의 크기와 위치 계산.
- **Painting:** `div`를 빨간색으로 화면에 그림.
- **Compositing:** 화면에 최종적으로 표시.
</aside>

## 프리로드 스캐너**(Preload scanner)**

- 브라우저는 **HTML을 순차적으로 파싱**하면서 요소를 해석합니다.
- 하지만, **렌더링 속도를 높이기 위해 "프리로더 스캐너"가 별도로 동작**하여 미리 리소스를 다운로드합니다.
- 즉, **HTML을 해석하는 파서(Parser)와는 별도로 동작하며, 페이지가 빠르게 로드될 수 있도록 돕는 역할**을 합니다.

### **역할**

1. **HTML 파서가 `<script>` 실행으로 멈춰도 CSS, 이미지 등의 다운로드를 미리 시작**
2. **웹페이지의 첫 화면을 더 빠르게 렌더링할 수 있도록 지원**
3. **네트워크 요청을 병렬로 수행하여 대기 시간을 줄임**
4. **특정 요소(`<link>`, `<img>` 등)가 존재하면, 이를 미리 요청하여 리소스 로딩을 가속화**

## HTML 렌더링 중 JavaScript가 실행되면 멈추는 이유

브라우저는 HTML 문서를 **위에서 아래로 순차적으로 파싱**하면서 웹페이지를 렌더링합니다. 하지만 **`<script>` 태그를 만나면** 렌더링을 멈추고 **JavaScript 파일을 다운로드 및 실행**한 후 다시 HTML을 파싱합니다.

1. **JavaScript는 DOM을 조작할 수 있음**
    - JavaScript가 DOM을 변경할 수도 있기 때문에, 브라우저는 **HTML을 모두 해석하기 전까지 JavaScript 실행을 멈추지 않음**.
2. **스크립트 실행이 끝나야 다음 HTML을 파싱할 수 있음**
    - `<script>` 내부에서 `document.write()` 같은 함수를 사용하면 **HTML 구조 자체가 변경될 가능성**이 있음.
3. **동기적 실행 방식**
    - 기본적으로 `<script>` 태그는 **HTML을 해석하는 동안 동기적으로 실행됨**.
    - 즉, JavaScript 실행이 끝나야만 브라우저는 다시 HTML을 파싱하고 렌더링을 진행함.

## **렌더링 속도를 늦추지 않는 방법**

### **1. `defer` 속성 사용 (추천!)**

- `defer` 속성을 사용하면 **HTML 파싱과 동시에 JavaScript 파일을 다운로드**하고,**HTML이 모두 파싱된 후에 실행**됩니다.
- 즉, 렌더링을 방해하지 않음.

```html
<script src="script.js" defer></script>
```

**특징**

- **HTML이 모두 로드된 후 실행됨** → 렌더링 차단 X
- **순서를 보장함** → 여러 개의 `defer` 스크립트는 순차적으로 실행됨.
- **DOM이 완전히 로드된 후 실행되므로, `DOMContentLoaded` 이벤트 전에 실행됨.**

### **2. `async` 속성 사용**

- `async` 속성을 사용하면 **HTML을 파싱하는 동안 JavaScript를 비동기적으로 다운로드**합니다.
- 하지만, 다운로드가 완료되면 즉시 실행되기 때문에 **HTML 파싱이 중단될 수 있음**.

```html
<script src="script.js" async></script>
```

**특징**

- **HTML을 파싱하면서 JavaScript 다운로드 가능** → 렌더링 차단 X
- **다운로드가 끝나는 즉시 실행됨** → 실행 순서가 보장되지 않음.
- **`DOMContentLoaded` 이벤트보다 먼저 실행될 수도 있음**.

**`async`는 실행 순서를 보장하지 않기 때문에, 스크립트 간 의존성이 있다면 `defer`가 더 적절합니다.**

### **3. `<body>` 태그 끝부분에 `<script>`**

- JavaScript 파일을 **`<body>` 끝부분**에 두면 **HTML이 완전히 렌더링된 후 실행**됩니다.
- 과거에는 이 방법이 많이 사용되었지만, **지금은 `defer`가 더 효과적인 대안**이 됨.

```html
<html>
<head>
  <title>Test</title>
</head>
<body>
  <h1>안녕하세요</h1>
  <script src="script.js"></script> <!-- HTML이 다 파싱된 후 실행 -->
</body>
</html>

```

**📌 특징**

- 렌더링을 방해하지 않음.
- **하지만 여러 개의 스크립트가 있으면 순차 다운로드됨** → 병렬 로딩이 되지 않아 비효율적일 수 있음.

## **크로스 브라우징(Cross-Browsing)**

**크로스 브라우징(Cross-Browsing)** 은 **모든 웹 브라우저(Chrome, Edge, Firefox, Safari 등)에서 동일한 웹페이지가 정상적으로 동작하도록 하는 기술**을 의미합니다.
즉, 특정 브라우저에서만 정상적으로 보이고, 다른 브라우저에서는 깨지는 현상을 방지하는 것이 목적입니다.

### 왜 크로스 브라우징이 필요한가?

1. 브라우저별 렌더링 엔진 차이
    1. 브라우저마다 **HTML, CSS, JavaScript 해석 방식이 다를 수 있음**.
2. CSS 속성 지원 차이
    
    **벤더 프리픽스 필요 (`-webkit-`, `-moz-`, `-o-`, `-ms-`)**
    
    ```css
    .box {
      -webkit-border-radius: 10px;
      -moz-border-radius: 10px;
      border-radius: 10px;
    }
    ```
    
3. JavaScript 엔진 차이
4. HTML 태그 지원 차이

### 해결방법

1. 표준 기술(HTML, CSS, JS) 사용
2. CSS 벤더 프리픽스 사용
3. JavaScript 호환성 처리 (Babel, Polyfill 사용)
    1. ES6+ 문법을 지원하지 않는 브라우저를 위한 폴리필(Polyfill) 사용
    2. `Babel`을 사용하면 구형 브라우저에서도 최신 JavaScript를 사용할 수 있도록 변환 가능.