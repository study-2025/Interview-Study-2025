[블로그 보러가기: Critical Rendering Path (CRP) 알아보기](https://velog.io/@ieun32/Critical-Rendering-Path-알아보기)

# 브라우저의 주요 기능

- 브라우저의 주요 기능은 사용자가 선택한 자원을 서버에 요청하고 브라우저에 표시하는 것이다. 자원은 보통 HTML 문서이지만 PDF나 이미지 또는 다른 형태일 수 있다. 자원의 주소는 URI(Uniform Resource Identifier)에 의해 정해진다.
- 브라우저는 HTML과 CSS 명세에 따라 HTML 파일을 해석해서 표시하는데 이 명세는 웹 표준화 기구 W3C(World Wide Web Consortium)에서 정한다. 과거에는 표준을 잘 따르지 않아 호환성 문제가 있었지만 최근에는 대부분의 브라우저가 표준을 따른다.

# 브라우저의 기본 구조

- 브라우저의 주요 구성은 다음과 같다.  
   ![브라우저 주요 구성](https://velog.velcdn.com/images/ieun32/post/3dfc2a67-23f1-4084-bc0a-26401528945e/image.png)

1. **사용자 인터페이스**  
   주소 표시줄, 이전 / 다음 버튼, 북마크 메뉴, 서치바 등 요청한 페이지를 보여주는 창을 제외한 나머지 모든 부분을 말한다.
2. **브라우저 엔진**  
   사용자 인터페이스와 렌더링 엔진 사이의 동작을 제어한다. 예를 들어 사용자가 이전 버튼을 누르면 브라우저 엔진이 이 이벤트를 감지해 렌더링 엔진에게 전달하여 화면을 변경하도록 한다.
3. **렌더링 엔진**  
   요청한 컨텐츠를 화면에 그려서 표시하는 역할을 한다. 예를 들어 HTML을 요청하면 HTML과 CSS를 파싱하여 화면에 표시한다. 브라우저마다 다른 렌더링 엔진을 사용한다. (크롬-블링크(웹킷파생), 사파리-웹킷 등)
4. **통신**  
   HTTP 요청과 같은 네트워크 호출에 사용된다. 이것은 플랫폼 독립적인 인터페이스이고, 각 플랫폼 하부에서 실행된다.
5. **UI 백엔드**  
   콤보 박스와 창 같은 기본적인 장치를 그린다. 플랫폼에서 명시하지 않은 일반적인 인터페이스로서, OS 사용자 인터페이스 체계를 사용한다.
6. **자바스크립트 해석기**  
   자바스크립트 코드를 해석하고 실행한다. 컴파일러라고도 부른다. 마찬가지로 브라우저마다 서로 다른 컴파일러를 사용한다.  
   (크롬 - v8, 사파리 - 자바스크립트 코어 등)
7. **자료 저장소 (브라우저 저장소, Web Storage)**  
   브라우저에 필요한 자료를 저장하는 계층이다. 로컬 스토리지, 세션 스토리지, 쿠키 등이 있다.

# Critical rendering path란?

- 말 그대로 직역하면 **주요 렌더링 경로** 라는 뜻으로, 유저가 웹페이지를 방문하고자 서버로 요청을 보내고 HTML, CSS, JS 등의 파일을 받아오고 나서 **브라우저가 화면을 보여주기 위해서 거치는 일련의 과정**을 말한다.
- 그러면 브라우저가 화면을 그릴 때는 어떤 과정을 거칠까? 어떻게 파일들을 분석해서 화면을 그려주는 것일까?
- **`*MDN web docs*`**에서는 아래와 같이 설명하고 있다.

> 주요 렌더링 경로 (_Critical Rendering Path_)는 브라우저가 HTML, CSS, Javascript를 화면에 픽셀로 변화하는 일련의 단계를 말하며 이를 최적화하는 것은 렌더링 성능을 향상시킵니다. 주요 렌더링 경로는 [Document Object Model](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model) (DOM), [CSS Object Model](https://developer.mozilla.org/ko/docs/Web/API/CSS_Object_Model) (CSSOM), 렌더 트리 그리고 레이아웃을 포함합니다.

![imqa.io를 주소창에 검색하면 일어나는 일, 출처: https://blog.imqa.io/webpage_loading_process/](https://velog.velcdn.com/images/ieun32/post/17d5bf29-a98e-4426-9d47-0cb1c6f7f6df/image.png)

imqa.io를 주소창에 검색하면 일어나는 일, 출처: [https://blog.imqa.io/webpage_loading_process/](https://blog.imqa.io/webpage_loading_process/)

- MDN 문서에서 설명한 바와 같이 브라우저가 화면을 그릴 때는 다음 단계를 거쳐야한다.
  1. **문서 객체 모델(DOM) 생성**
  2. **CSS 객체 모델(CSSOM) 생성**
  3. **자바스크립트 파싱**
  4. **렌더 트리 생성**
  5. **레이아웃**
  6. **페인트**
  7. **컴포지트**
- 이때 이 과정을 수행해주는 브라우저의 구성 요소가 바로 **렌더링 엔진**이다.
- 그러면 각 단계에서 무엇을 수행하는 지 구체적으로 알아보자.

## 1. Document Object Model

- 사용자가 URL을 사용해 서버에 데이터를 요청하면 HTTP 응답을 받는다. 응답은 start-line, header, body 의 세 부분으로 구성되는데 본문에는 HTML 바이너리 데이터가 포함된다.
- 브라우저가 HTML을 응답 본문으로 받으면 HTML 처리 및 DOM 구축을 하기 시작한다.
  - 이때 DOM이란, HTML문서를 브라우저가 이해할 수 있도록 만든 Tree 자료구조를 말한다. DOM은 계층 구조를 가지고 있으며 자바스크립트로 DOM에 접근해 직접 조작할 수 있다.
- 브라우저가 HTML 처리 및 DOM 구축을 하는 과정은 다음과 같다.  
   ![](https://velog.velcdn.com/images/ieun32/post/a3fab136-23cf-436f-8057-7a525c0c2e04/image.png)
  1. **바이트를 문자로 변환**

     HTTP로 요청하고 응답을 받을 때는 데이터가 바이트 단위로 [직렬화(Serialization)](https://developer.mozilla.org/ko/docs/Glossary/Serialization) 되어서 도착한다. 따라서 이를 다시 문자로 변환하는 역직렬화 과정을 거친다.

  2. **토큰 식별**  
     문자로 변환된 HTML 파일을 토크나이저를 통해 토큰으로 변환한다. 토큰으로 변환된 HTML 파일은 다음과 같은 모습을 하고 있다.  
     `StartTag: head Tag: meta Tag: link EndTag: head Hello…`
  3. **토큰을 노드로 변환**  
     토크나이저가 이 작업을 수행하는 동안 다른 프로세스가 이러한 토큰을 사용해 노드 객체로 변환한다.
  4. **DOM 트리 구축**  
     모든 토큰을 사용하면 HTML의 콘텐츠와 속성과 모든 항목을 캡쳐하는 트리 구조인 DOM에 도달한다.
- 브라우저는 DOM을 점진적으로 구성한다. 즉 브라우저는 처리를 시작하기 전에 모든 HTML이 서버에서 도착할 때까지 기다릴 필요가 없으므로 이 프로세스를 활용하여 속도를 높일 수 있다.
- _DOM이 구성되는 과정을 실시간으로 트리 형태로 보고 싶다면 아래 사이트를 참고하자.  
   [https://software.hixie.ch/utilities/js/live-dom-viewer/#](https://software.hixie.ch/utilities/js/live-dom-viewer/#)_

## 2. CSS Object Model

![](https://velog.velcdn.com/images/ieun32/post/3be874aa-9818-4891-9449-d09335259800/image.png)

- 렌더링 엔진은 HTML 파일을 읽으면서 DOM 트리를 생성해나가다가 CSS를 로드하는 link 태그나 style 태그를 만나면 DOM 생성을 일시 중단하고, CSSOM 트리를 생성한다. CSS 파싱이 완료되면 HTML 파싱이 중단된 시점부터 다시 HTML 파싱을 시작한다
- CSS의 파싱 또한 HTML 파싱 과정과 동일한 과정을 거친다.
  - CSS 바이트 파일 인코딩 ⇒ 문자 토큰화 ⇒ 토큰 노드화 ⇒ 노드 모델 트리화
- CSSOM에는 CSS 선택자와 관련된 속성을 기록하게 된다.
  - 위 그림과 같이 body 태그에는 font-size를 16으로 설정했다는 정보를 기록
- CSS를 만났을 때 DOM 생성을 멈추는 이유는 무엇일까?
  - 예를 들어 `{background: 'blue'}` 이후에 `{background: 'red'}` CSS 코드가 있다고 가정해보자.
  - 브라우저가 `{background: 'blue'}` 부분만 수신하여 CSSOM을 구성하고 바로 렌더링 한다면 개발자가 의도한 것은 빨간색 배경화면을 표시하는 것인데 파란색 배경화면으로 표시될 수가 있다.
  - 이런 현상을 방지하기 위해 브라우저는 모든 CSS를 수신하고 처리할 때까지 페이지 렌더링을 차단한다.

## 3. JavaScript Parsing

![출처: https://ghost4551.tistory.com/220](https://velog.velcdn.com/images/ieun32/post/f1ce0ced-f033-4a73-b17a-0a00ff7d756c/image.png)

출처: [https://ghost4551.tistory.com/220](https://ghost4551.tistory.com/220)

- 렌더링 엔진은 CSS 파싱과 비슷하게 자바스크립트 파일을 로드하는 script 태그나 자바스크립트 코드를 만나면 DOM 생성을 **일시 중단**한다. 그리고 자바스크립트 파일을 파싱하기 위해 자바스크립트 엔진에게 제어권을 넘긴다.
- 렌더링 엔진으로부터 제어권을 넘겨받은 자바스크립트 엔진은 자바스크립트 코드를 파싱하기 시작한다. 렌더링 엔진이 DOM과 CSSOM을 생성하듯 자바스크립트 엔진은 자바스크립트를 해석하여 `AST`(추상적 구문 트리)를 생성한다. 그리고 `AST`를 기반으로 인터프리터가 실행할 수 있는 중간 코드인 바이트코드를 생성하여 실행한다.
- 이때 script 태그가 html 문서 중간에 있으면, script 태그 아래에 있는 DOM은 참조, 조작할 수 없다. 따라서 script 태그를 html 문서 가장 아래에 두거나 defer, async 속성을 줄 수 있다.

## 4. Render Tree

![Render Tree](https://velog.velcdn.com/images/ieun32/post/0b3c2315-7076-4499-a5b3-da7f2da11e4c/image.png)

출처: [https://dev.to/coderedjack/critical-rendering-path-web-performance-23ij](https://dev.to/coderedjack/critical-rendering-path-web-performance-23ij)

- DOM과 CSSOM이 구성된 후에는 두 객체 모델을 결합해서 렌더 트리를 형성한다.
- 이때 렌더 트리는 보이는 컨텐츠만 캡쳐한다.
  - 즉 `display: none` 과 같은 속성이 있는 요소는 무시한다.

### 4-1. Render Blocking resources

![](https://velog.velcdn.com/images/ieun32/post/d464af10-c964-4518-b8ff-666a85c6f05b/image.png)  
출처: [https://blog.imqa.io/webpage_loading_process/](https://blog.imqa.io/webpage_loading_process/)

- 위에서 언급한 것처럼 CSS와 JS파일은 DOM 생성을 중단하고, CSSOM의 생성은 JS의 실행을 멈추기도 한다.
- 따라서 CSS와 JS 파일이 너무 많거나 크면 Render Tree를 만드는 시점이 지연되어 웹 로딩 시간도 늦어지게 된다. 그래서 이러한 render-blocking resources를 최대한 제거하거나 **최적화하는 것은 웹 성능을 위해서 중요한 작업**이다.

## 5. Layout

![](https://velog.velcdn.com/images/ieun32/post/c1abf7c1-ee14-421d-be2b-c766e29e3240/image.png)

출처: [https://dev.to/coderedjack/critical-rendering-path-web-performance-23ij](https://dev.to/coderedjack/critical-rendering-path-web-performance-23ij)

- **_Layout_**은 웹페이지 기준으로 **각 요소의 위치와 크기를 결정하는 작업**이다. 윈도우의 크기, Render Tree에서 구했던 스타일을 참고해서 요소의 실제 기하학적 속성을 결정한다.
  - 예를 들어서 CSS 속성으로 div 태그를 화면 너비의 50%로 표현해달라고 했다면 현재 화면 너비 픽셀에서 50%가 몇 픽셀인지 계산한다.
- 이때 요소의 너비, 높이, 위치를 변경할 때마다 브라우저는 레이아웃 단계를 실행한다. 이를 Reflow라고 한다. 반응형 디자인을 적용했다면 레이아웃 단계가 자주 일어날 것이다. Reflow가 자주 일어나면 뒤따르는 Paint와 Composite까지 재진행 하기 때문에 계산이 가장 많이 드는 작업이다.

## 6. Paint

![](https://velog.velcdn.com/images/ieun32/post/6cf1ed78-10db-4b79-a274-5e6e347662a5/image.png)

- 이제 *Layout*으로 요소들의 실제 크기와 위치를 정했다면, *Paint* 단계에서 실제 픽셀 단위로 색칠을 한다.
- **_Paint_**는 텍스트, 색상, 테두리, 그림자 등 요소의 **모든 시각적 부분을 화면에 그리는 작업**이다.
- 이때 작업에는 래스터라이저를 통해 벡터를 래스터로 변환하는 작업이 포함된다.
  - 래스터라이저는 *save, translate, drawRectangle, drawText, clipPath* 등과 같은 그리기 호출을 사용해서 픽셀을 채운다.
- 브라우저는 Paint 과정을 매우 빠르게 진행하기 때문에 성능에서 크게 중요한 부분은 아니다.

## 7. Composite

![](https://velog.velcdn.com/images/ieun32/post/2a93c3cc-f106-4122-95dc-9772bd7de3f7/image.png)

출처: 이미지 속 기재

- Layout 과정 중 HTML, CSS 속성에 따라서 요소들이 서로 다른 레이어에 그려질 때가 있다.
- 보통 HTML의 `<video>`, `<canvas>` 태그나 CSS의 position, z-index, 3D 속성에 의해 레이어가 생성되는데, 이러한 이유로 레이어가 2개 이상 생성될 때 **각 레이어를 하나로 합치는 과정**을 **_Composite_**라고 한다.
- Paint 과정은 단순히 요소를 색칠하는 과정이었다면 Composite 과정은 요소들을 정확한 순서로 화면에 그리는 작업이다. 특히 요소들이 겹칠 때 이 작업은 굉장히 중요하다.

이렇게 Composite 과정까지 마치면 웹 페이지 로딩이 완료된다.

# Reflow, Repaint

## Reflow

- width, heigth, position 등과 같은 위치나 크기가 변경될 때 Layout 과정을 다시 거치게 된다. Reflow가 발생하면 필연적으로 Repaint, Composite 과정도 거쳐야한다. 이러한 과정을 **`*Reflow*`**라고 한다.

## Repaint

- color, background-color, outline 등을 변경하면 Layout 과정을 생략하고 바로 Paint 과정을 거치는데 이러한 과정을 **`*RePaint*`**라고 한다.
- Reflow와 Repaint가 일어나게 하는 CSS 트리거들이 있다. 이는 아래 두 링크에서 확인 가능하다. 이때 렌더링 엔진별로 동작이 다르기 때문에 유의하도록 한다.

[CSS Triggers List - What Kind of Changes You Can Make](https://csstriggers.com/)

- Reflow와 Repaint 작업이 너무 자주 일어나게 되면 웹 성능에 영향을 미친다. 사용자 입장에서는 동적인 요소를 볼 때 초당 60프레임이 제공되어야 자연스럽다고 느끼는데, 브라우저가 너무 자주 일어나는 Reflow와 Repaint 때문에 60fps 속도로 프레임을 제공해주지 못하면 사용자 입장에서는 버벅이는 화면을 보게 된다.
- 모던 웹에서는 정적 사이트보다 동적 사이트가 많기 때문에 Reflow, Repaint가 일어나는 것을 아예 차단할 수는 없겠지만, 두 작업 모두 최대한 일어나지 않게끔 최적화를 하는 것이, 특히 더 비용이 많이 들어가는 Reflow 작업을 일어나지 않게 해야 웹 성능을 향상 시킬 수 있을 것이다.

그렇다면 Reflow와 Repaint를 최소화 할 수 있는 방법은 무엇일까?

- Paint Layer와 Graphics Layer를 활용한다.
- Paint Layer는 Stacking Context를 이루는 층을 말한다. z축을 기준으로 쌓이는 하나하나의 층이다.
- Paint Layer를 활용하면 기존 요소의 위치를 재계산할 필요 없이 층만 쌓으면 된다.
- Graphics Layer는 화면을 렌더링 할 때 GPU(Graphics Processing Unit)의 도움을 얻는 층을 말한다. GPU의 도움을 받으면 CPU가 혼자 계산하던 렌더링 연산을 나눠서 할 수 있으므로 성능 개선에 도움을 준다.
- **Paint Layer를 만드는 트리거**는 다음과 같다.
  1. 문서의 루트요소인 `<html>`
  2. position이 absolute 또는 relative이고, z-index가 auto가 아닐때.
  3. position이 fixed 또는 sticky인 요소
  4. 플렉스(flexbox) 컨테이너의 자식 요소 중 z-index가 auto가 아닐때.
  5. 그리드(grid) 컨테이너의 자식 요소 중 z-index가 auto가 아닐때.
  6. **opacity가 1보다 작을 때**
  7. mix-blend-mode가 normal이 아닐 때.
  8. 다음 속성 중 하나라도 none이 아닐 때 : transform, filter, perspective, clip-path, mask
  9. isolation이 isolate일 때.
  10. -webkit-overflow-scrolling이 touch일 때.
  11. will-change의 값으로 초기값이 아닐 때 새로운 Stacking Context를 생성하는 속성을 지정했을 경우
  12. contain이 layout, paint 또는 둘 중 하나를 포함하는 값(strict, content등)인 경우
  13. 화면에 보이지 않지만 층을 이룰 때
- **Graphics Layer 트리거**들은 다음과 같다.
  1. `<video>` 혹은 `<canvas>` 태그 사용
  2. 3D transform 요소 적용
  3. **animation 이나 transition 사용**
  4. will-change로 opacity, transform, top, left, bottom, right등이 정의된 경우
  5. iFrame일 경우
  6. flash와 같은 일부 플러그인
  7. backface-visibility 가 hidden일 경우
- 따라서 left, right 대신 transform 속성을 사용하고 visibility, display 대신 opacity 속성을 사용하면 reflow, repaint 발생을 줄일 수 있다는 말이 있는 것이다.

# Reference

[웹 페이지 로딩 과정 이해하기](https://blog.imqa.io/webpage_loading_process/)

[브라우저의 작동 방식  |  Articles  |  web.dev](https://web.dev/articles/howbrowserswork?hl=ko)

[](https://developer.mozilla.org/en-US/docs/Web/Performance/Critical_rendering_path)

[Ryan Seddon: So how does the browser actually render a website | JSConf EU 2015](https://www.youtube.com/watch?v=SmE4OwHztCc&t=164s)

[Critical Rendering Path (Web Performance)](https://dev.to/coderedjack/critical-rendering-path-web-performance-23ij)

[[HTML] DOM이란 무엇일까?](https://velog.io/@sj_dev_js/HTML-DOM%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)

[CSSOM(CSS Object Model) 이란?](https://itworldyo.tistory.com/entry/CSSOMCSS-Object-Model-%EC%9D%B4%EB%9E%80)

[웹개발자면서 이것도 모름? | DOM과 CSSOM, 렌더링 과정](https://www.youtube.com/watch?v=Mqh13dNI8jc)

[[JavaScript DeepDive] 38장\_브라우저의 렌더링 과정](https://ghost4551.tistory.com/220)

[[React] Reflow와 Repaint](https://velog.io/@dmstmdrbs/React-Reflow%EC%99%80-Repaint)

[-TIL/성능 최적화/Reflow, Repaint.md at master · FE-Lex-Kim/-TIL](https://github.com/FE-Lex-Kim/-TIL/blob/master/%EC%84%B1%EB%8A%A5%20%EC%B5%9C%EC%A0%81%ED%99%94/Reflow%2C%20Repaint.md)

[[CSS] opacity는 reflow가 발생 안 한다구요...? 정말??](https://blinders.tistory.com/93)
