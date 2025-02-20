# 4. CSS

## 1) CSS 우선순위

여러 개의 스타일이 한 요소에 적용될 수 있다 ⇒ **어떤 스타일이 최종적으로 적용될지 결정 기준 필요**

그 기준이 CSS 우선순위다.

우선순위는 **선택자의 유형**에 따라 점수를 부여하여 결정된다.

가장 높은 점수를 가지는건 `!important` 인데 비사용 권장

| 선택자 유형                             | 예제                              | 점수   |
| --------------------------------------- | --------------------------------- | ------ |
| **인라인 스타일**                       | `<div style="color: red">`        | `1000` |
| **ID 선택자**                           | `#header`                         | `100`  |
| **클래스, 속성, 가상 클래스 선택자**    | `.box`, `[type="text"]`, `:hover` | `10`   |
| **태그(요소) 선택자, 가상 요소 선택자** | `div`, `p`, `::before`            | `1`    |
| **전체 선택자, 상속된 스타일**          | `*`, `color: inherit;`            | `0`    |

```css

/* 1점 (태그 선택자) */
p {
  color: blue;
}

/* 10점 (클래스 선택자) */
.text {
  color: green;
}

/* 100점 (ID 선택자) */
#main {
  color: red;
}

/* 1000점 (인라인 스타일) */
<p id="main" class="text" style="color: black;">Hello</p>

```

**결과**: `color: black;`

같은 점수라면 코드에서 마지막에 선언된 스타일이 최종 적용됨.

## 2) Reset css vs. Nomalize css

브라우저마다 CSS 스타일이 다르게 적용됨.
이를 통일하기 위해 초기화(Reset) 또는 표준화(Normalize)가 필요함

### **Reset CSS**

**브라우저의 기본 스타일을 완전히 제거**

```css
* {
  margin: 0;
  padding: 0;
  border: 0;
  box-sizing: border-box;
}
```

→ 모든 요소 **초기 값 제거 (ul, ol, table 도 마찬가지)**

- 개발자가 모든 스타일을 직접 지정해야 함
- 오히려 추가 작업이 많아질 수 있음

### **Normalize CSS**

**브라우저 스타일을 일관성 있게 정리**

```css
html {
  line-height: 1.15;
}

body {
  margin: 0;
}

h1 {
  font-size: 2em;
  margin: 0.67em 0;
}
```

→ 기본 마진, 패딩을 조정하지만 완전히 제거하지 않음

→ `<h1>`, `<p>`, `<button>` 등 브라우저마다 다르게 보이는 요소 통일

- 필요 이상으로 스타일을 덮어쓰지 않으므로 추가적인 작업이 적음
- 유지보수가 쉬워 최근 프로젝트에서는 Normalize CSS가 더 선호됨

## 3) **CSS Modules / CSS Variables / CSS in JS**

### CSS

**기본적인 CSS 방식**

- HTML 파일에서 `<link rel="stylesheet" href="styles.css">`로 사용 가능
- 전역 스타일링이 기본이므로 네임스페이스 충돌 가능성 있음

| 장점                                                  | 단점                                           |
| ----------------------------------------------------- | ---------------------------------------------- |
| **브라우저 호환성 100%**                              | 전역 스타일이 기본 → 유지보수 어려움           |
| 별도 설정 없이 바로 사용 가능                         | **재사용성이 떨어짐** (변수, 모듈화 기능 부족) |
| **런타임 성능이 가장 좋음 (파일 로드만 하면 되니까)** |                                                |

<aside>
💡

이러한 CSS의 단점으로 CSS **모듈화, 변수화, 컴포넌트화** 할 수 있는 방식들이 생겨남.

</aside>

### **CSS Modules**

**CSS를 모듈화하여 관리하는 방식**

**→ 클래스명이 자동으로 고유화되어 충돌을 방지**

```css
/* styles.module.css */
.button {
  background-color: blue;
  color: white;
  padding: 10px;
  border-radius: 5px;
}
```

```jsx
import styles from "./styles.module.css";

function Button() {
  return <button className={styles.button}>Click me</button>;
}
```

클래스명이 **자동으로 고유하게 변환됨 →** 클래스명이 중복될 위험 없음

| 장점                                | 단점                        |
| ----------------------------------- | --------------------------- |
| \*\*\*\*고유화되어 충돌 없음        | JS 파일에서 `import`해야 함 |
| 기존 CSS 문법을 그대로 사용 가능    | 동적 스타일 변경이 어려움   |
| 성능 좋음 (JS 대신 CSS 파일을 사용) |                             |

### **CSS Variables**

**CSS에서 전역 또는 지역적으로 변수를 선언하여 사용**

```css
:root {
  --main-color: blue;
  --padding: 10px;
}

.button {
  background-color: var(--main-color);
  padding: var(--padding);
  color: white;
}
```

→ `:root`에 선언하면 전역 변수로 활용 가능

장점 : 테마 변경, 다크 모드 적용이 쉬움 / 생산성 증가

단점 : 구형 브라우저 지원 부족

### **CSS-in-JS**

**JS 코드 내에서 스타일을 작성하는 방식**

```jsx
import styled from "styled-components";

const Button = styled.button`
  background-color: blue;
  color: white;
  padding: 10px;
  border-radius: 5px;
`;

function App() {
  return <Button>Click me</Button>;
}
```

→ 스타일을 컴포넌트 단위로 사용 가능

| 장점                                         | 단점                                           |
| -------------------------------------------- | ---------------------------------------------- |
| JS 변수 및 Props를 활용한 동적 스타일링 편리 | 런타임 성능 저하 (JS가 실행될 때 CSS가 생성됨) |
| 유지보수/ 재사용성                           | SSR 지원이 어려울 수 있음 (스타일 로딩 문제)   |

## 4) CSS 비교

### SASS(SCSS)

**CSS의 단점을 보완한 전처리기**

- **변수, 중첩, 확장성 등의 기능 제공**

```scss
$primary-color: blue;

.button {
  background-color: $primary-color;
  color: white;
}
```

| 장점                                            | 단점                                                       |
| ----------------------------------------------- | ---------------------------------------------------------- |
| CSS보다 재사용성, 유지보수성 좋음               | 컴파일 과정이 필요 (`node-sass`, `dart-sass` 사용)         |
| 변수(`$var`), 중첩(`&`), Mixin 활용             | CSS 파일을 전역적으로 관리 (컴포넌트별 스타일 분리 어려움) |
| 기존 CSS 문법과 100% 호환 (`.scss` 확장자 사용) | CSS-in-JS 대비 동적 스타일링이 불편                        |

### **Styled Components**

**JS 코드 내에서 스타일을 직접 작성하는 방식**

```jsx
import styled from "styled-components";

const Button = styled.button`
  background-color: blue;
  color: white;
`;

function App() {
  return <Button>Click me</Button>;
}
```

| 장점                          | 단점                                                  |
| ----------------------------- | ----------------------------------------------------- |
| **동적 스타일링 용이**        | **JS 실행 시 CSS가 생성 → 성능 저하 가능**            |
| CSS를 **컴포넌트별로 캡슐화** | 런타임에 스타일이 적용되므로 초기 로딩 속도 저하 가능 |

### **Tailwind CSS**

**유틸리티 퍼스트 방식의 CSS 프레임워크**
(유틸리티 퍼스트 : CSS를 거의 사용하지 않고 HTML에서 스타일을 조합)

```jsx
<button className="bg-blue-500 text-white px-4 py-2 rounded">Click me</button>
```

→ 별도의 CSS 파일을 작성할 필요 없이 클래스만으로 스타일 적용 가능

| 장점                            | 단점                                    |
| ------------------------------- | --------------------------------------- |
| 빠른 개발 가능                  | 클래스 네이밍이 길어질 수 있음          |
| 전역 스타일 충돌X               | 기존 CSS 문법과 다르므로 러닝 커브 필요 |
| 반응형, 다크 모드 지원이 강력함 | 유지보수가 어려울 수 있음               |
