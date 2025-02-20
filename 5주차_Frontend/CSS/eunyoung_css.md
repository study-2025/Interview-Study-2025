# CSS

## 1️⃣ resetCSS vs nomalize CSS

### User Agent Stylesheet

CSS로 스타일링을 하다보면 **내가 설정하지 않은 속성과 값이 적용되어 있는 것**을 볼 수 있다. 이는 브라우저마다 HTML 요소를 렌더링 할 때 기본적으로 가지고 있는 **`User Agent Stylesheet`**가 있기 때문이다.

**`User Agent Stylesheet`**란, 브라우저가 자체적으로 설정해놓은 기본 스타일링을 모아놓은 것을 말한다. DOCTYPE을 선언하지 않거나, 따로 CSS 파일을 제공하지 않는 경우 적용된다. (아무런 CSS값을 지정해주지 않으면 적용된다)

이렇듯 브라우저마다 정의하는 기본 스타일이 모두 다르기 때문에, 사용자에게 일관된 화면을 전달하기 위해서는, 즉 **크로스 브라우징 이슈를 해결하기 위해서는 브라우저별 기본 스타일링을 초기화해줄 필요**가 있다.

어떻게 초기화 할 수 있을 지 알아보자.

### 초기화 하는 방법 (reset vs normalize)

초기화 하는 방법은 간단하다. CSS는 Cascading, 상속되므로 브라우저가 기본적으로 정의해놓은 `User Agent Stylesheet` 위에 **새로운 스타일링을 덮어 씌우는 방법**으로 해결할 수 있다.

보편적으로 두 가지 방법을 사용하는데,

- **reset (초기화)**
- **normalize (표준화)**

방법으로 구분할 수 있다.

![[https://velog.io/@nalsae/내보정CSS-reset-줄까-normalize-줄까](https://velog.io/@nalsae/%EB%82%B4%EB%B3%B4%EC%A0%95CSS-reset-%EC%A4%84%EA%B9%8C-normalize-%EC%A4%84%EA%B9%8C)](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2b6748e-ba74-4d54-a235-94c36f9d61cb/73ae9e35-fbd3-4d46-9c6d-4b97ab21ef91/Untitled.png)

[https://velog.io/@nalsae/내보정CSS-reset-줄까-normalize-줄까](https://velog.io/@nalsae/%EB%82%B4%EB%B3%B4%EC%A0%95CSS-reset-%EC%A4%84%EA%B9%8C-normalize-%EC%A4%84%EA%B9%8C)

위 그림을 통해 `reset` 과 `normalize`의 컨셉을 알아보자.

그림 속 내용을 번역하면, **브라우저마다 서로 다른 스타일링이 있을 때**

- **`reset`**은 **기본 스타일의 대부분을 제거**하고 새롭게 정의하는 스타일을 조금 추가한다.
- **`normalize`**는 브라우저마다 **일관되지 않는 스타일링만 제거**한다.

위 그림에서 컵에 남아 있는 물의 양을 봐도 `reset` 이 제거하는 스타일링이 훨씬 많은 것을 알 수 있다.

여기서 생각해보자. 제거하는 스타일링이 많다는 것은 당연히 CSS가 수행하는 계산이 많다는 뜻이다. 제거라고 표현했지만 사실 실질적으로 스타일링을 삭제하는 것이 아니라, **덮어씌우는 방식으로 제거된 것처럼 보이게 하는 것**이기 때문이다.

결국 `reset`은 `normalize`보다 덮어씌우는 스타일이 많아지기 때문에 성능이 저하될 수 있다.

다시 정리하면 둘의 특징은 다음과 같다.

### reset CSS

- 브라우저마다 다르게 설정된 요소의 **기본 스타일링을 모두 초기화**하는 방식
- 크로스 브라우징을 위해 필요한 속성을 임의로 지정하여 파일로 통합한 것

### 사용 방법 ① : cdn 이용하기

index.html의 <head> 태그 안에 `cdn`(content delivery network)을 import 해준다.

```html
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/reset-css@4.0.1/reset.min.css"
/>
```

### 사용 방법 ②: reset.css 파일 만들기

아래 홈페이지에 들어가서 css 코드 전체를 복사하여 reset.css 파일에 붙여 넣고 적용해준다.

[meyerweb.com](https://meyerweb.com/eric/tools/css/reset/)

제공되는 reset.css 코드

```css
/* http://meyerweb.com/eric/tools/css/reset/ 
   v2.0 | 20110126
   License: none (public domain)
*/

html,
body,
div,
span,
applet,
object,
iframe,
h1,
h2,
h3,
h4,
h5,
h6,
p,
blockquote,
pre,
a,
abbr,
acronym,
address,
big,
cite,
code,
del,
dfn,
em,
img,
ins,
kbd,
q,
s,
samp,
small,
strike,
strong,
sub,
sup,
tt,
var,
b,
u,
i,
center,
dl,
dt,
dd,
ol,
ul,
li,
fieldset,
form,
label,
legend,
table,
caption,
tbody,
tfoot,
thead,
tr,
th,
td,
article,
aside,
canvas,
details,
embed,
figure,
figcaption,
footer,
header,
hgroup,
menu,
nav,
output,
ruby,
section,
summary,
time,
mark,
audio,
video {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font: inherit;
  vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section {
  display: block;
}
body {
  line-height: 1;
}
ol,
ul {
  list-style: none;
}
blockquote,
q {
  quotes: none;
}
blockquote:before,
blockquote:after,
q:before,
q:after {
  content: "";
  content: none;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
```

### normalize CSS

- reset CSS 방식과 동일하지만, **사용하기 좋은 기본 값들은 초기화하지 않고 유지**한다.
- 부분적으로만 개선하는 방식이기 때문에 **reset CSS 보다 성능 면에서 유리**하다.

### 사용 방법 ① : cdn 이용하기

reset.css 에서 cdn 방식을 사용하는 것과 동일한 방식으로 아래처럼 cdn을 index.html 태그 안에 import 한다.

```html
<link
  rel="stylesheet"
  href="https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.1/normalize.min.css"
/>
```

### 사용 방법 ② : npm install 하기

npm 패키지로 normalize.css 가 배포되어 있기 때문에 설치해서 사용할 수 있다.

[npm: normalize.css](https://www.npmjs.com/package/normalize.css?activeTab=versions)

### 어떤 걸 사용할까?

위에서 살펴본 것처럼 크로스 브라우징 이슈를 해결하기 위해서는 User Agent Stylesheet를 재정의해야 한다.

일반적으로 Normalize는 일관된 스타일을 보장하고 표준화된 스타일을 유지하는 데 도움이 되며, Reset은 초기화된 스타일을 완전히 커스터마이징할 수 있는 자유로운 접근 방식을 제공하기 때문에 아래 기준들처럼 **프로젝트의 요구 사항과 목표를 고려하여 선택**하는 것이 중요하다.

두 초기화 도구를 선택하는 기준은 아래와 같다.

1. 목표:
   - Reset: 모든 브라우저에서 일관된 스타일을 적용하기 위해 모든 기본 스타일을 제거한다.
   - Normalize: 각 브라우저의 기본 스타일을 표준화하고 일관성을 유지한다.
2. 요구사항:
   - Reset: 완전히 동일한 스타일을 갖는 것이 중요한 경우에 사용된다.
   - Normalize: 각 브라우저에서 일관된 모양과 동작을 보장하면서 기본 스타일을 일부 표준화해야 하는 경우에 사용된다.
3. 커스터마이징:
   - Reset: 초기화된 스타일을 완전히 커스터마이징할 수 있다.
   - Normalize: 표준화된 스타일을 일부 커스터마이징할 수 있지만, 기본적으로 브라우저 스타일을 유지한다.
4. 프로젝트 크기:
   - Reset: 작은 프로젝트나 프로젝트의 스타일을 완전히 커스터마이징해야 하는 경우에 적합하다.
   - Normalize: 대형 프로젝트이거나 크로스 브라우징 및 일관된 스타일이 필요한 경우에 적합하다.

## 2️⃣ CSS 우선순위

CSS로 스타일링을 하다 보면 의도한 대로 스타일이 적용되지 않는 경우가 많다. 이는 대부분 **_CSS 우선순위를 제대로 이해하지 못해서 발생하는 문제_**이다.

CSS 우선순위에 대해 알아보자.

### CSS 우선순위란?

**CSS 우선순위(Specificity)**는 여러 CSS 규칙이 동일한 요소에 적용될 때 어떤 규칙이 우선적으로 적용될지 결정하는 규칙이다.

### 우선순위 계산 방법

우선순위는 다음과 같은 순서와 점수로 계산된다.

| **우선순위** | **항목**                                                                            | **점수** |
| ------------ | ----------------------------------------------------------------------------------- | -------- |
| 1            | **`!important` (가장 높음)**                                                        | 무한대   |
| 2            | **인라인 스타일**                                                                   | 1000점   |
| 3            | **ID 선택자 (`#id`)**                                                               | 100점    |
| 4            | **클래스 선택자 (`.class`), 속성 선택자 (`[type="text"]`), 가상 클래스 (`:hover`)** | 10점     |
| 5            | **요소 선택자 (`div`, `p`), 가상 요소 (`::before`)**                                | 1점      |
| 6            | **전체 선택자 `*`**                                                                 |          |

### 우선순위 계산 예시

```jsx
/* 점수: 1 (요소 선택자) */
p { color: blue; }

/* 점수: 10 (클래스 선택자) */
.text { color: red; }

/* 점수: 100 (ID 선택자) */
#title { color: green; }

/* 점수: 11 (요소 선택자 + 클래스 선택자) */
p.text { color: yellow; }

/* 점수: 111 (요소 선택자 + 클래스 선택자 + ID 선택자) */
p.text#title { color: purple; }
```

### 자주 하는 실수들

**1. 과도한 !important 사용**

```css
/* 나쁜 예 */
.button {
  color: red !important;
  background: blue !important;
}

/* 좋은 예 */
.button.primary {
  color: red;
  background: blue;
}
```

**2. 불필요하게 구체적인 선택자**

```css
/* 나쁜 예 */
div.container .wrapper ul li a.link {
  color: blue;
}

/* 좋은 예 */
.nav-link {
  color: blue;
}
```

**3. ID 선택자 과다 사용**

```css
css
Copy
/* 나쁜 예 */
#header #nav #login-button {
  color: blue;
}

/* 좋은 예 */
.header .nav-button {
  color: blue;
}
```

### 우선순위 관리를 위한 best practices

1. **BEM 방법론 사용**

```css
.block__element--modifier {
  color: blue;
}
```

1. **클래스 기반 스타일링**

```css
/* 권장 */
.button.primary {
  background: blue;
}

/* 비권장 */
#submit-button {
  background: blue;
}
```

1. **계층 구조 최소화**

```css
/* 비권장 */
.header .nav .list .item .link {
  color: blue;
}

/* 권장 */
.nav-link {
  color: blue;
}
```
