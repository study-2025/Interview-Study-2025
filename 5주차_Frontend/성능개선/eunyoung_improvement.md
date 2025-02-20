# 최적화

## 1️⃣ 코드 스플리팅

### 코드 스플리팅(Code Splitting)이란?

우리가 JavaScript로 애플리케이션을 개발하게 되면, 기본적으로는 하나의 파일에 모든 로직들이 들어가게 된다. 그러면 프로젝트 규모가 커질수록 JavaScript 파일 용량도 함께 커지게 되고, 페이지 로딩 속도가 느려질 것이다.

따라서 지금 **당장 필요한 코드가 아니라면 따로 분리**시켜서, 나중에 필요할 때 불러와서 사용할 수 있도록 하는 것을 **코드 스플리팅**이라고 한다.

코드 스플리팅을 하는 방법은 다음과 같다.

### 1. Webpack의 Entry Points와 SplitChunkPlugin

`webpack.config.js` 구성 파일에 다음과 같이 `entry` 속성을 줄 수 있다.

key값은 개발자가 임의로 정할 수 있고, value에는 JavaScript 파일의 경로를 지정해주면 된다.

```jsx
// webpack.config.js
 const path = require('path');

 module.exports = {
  mode: 'development',
	**entry: {
		changes: "./src/js/pages/changes.page.js",
		productManage: ".src/js/pages/product-manage.page.js",
		app: "./src/js/app.js",
	},**
   output: {
    filename: '[name].bundle.js',
     path: path.resolve(__dirname, 'dist'),
   },
 };

```

위와 같이 entry 속성을 지정해주면,

왼쪽 그림과 같이 기존에 모든 JavaScript파일을 하나로 묶어 번들링 되었던 것이

오른쪽 그림과 같이 총 3개의 JavaScript파일로 분리되어 번들링 된다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2b6748e-ba74-4d54-a235-94c36f9d61cb/b93f3125-7104-432a-a213-6a3309a644d5/Untitled.png)

그러나 이 방식에는 다음과 같은 단점이 있다.

- entry chunk 사이에 중복된 모듈이 있는 경우 두 번들에 중복되어 모두 포함된다.
- core 애플리케이션 로직을 통한 코드의 동적 분할에는 사용할 수 없으며 유연하지 않다.

이때 모듈 중복을 제거할 수 있는 옵션을 지정해줄 수 있다.

`optimization.splitChunks` 설정 옵션을 적용하면 chunk간 중복을 제거하고 공통 의존성을 추출할 수 있다.

```jsx
// webpack.config.js
  const path = require('path');

  module.exports = {
    mode: 'development',
    entry: {
      index: './src/index.js',
      another: './src/another-module.js',
    },
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
   **optimization: {
     splitChunks: {
       chunks: 'all',
     },
   },**
  };
```

### 2. Dynamic import

Dynamic import를 사용하면 **런타임 시 필요한 module을 import할 수 있다. 즉 필요한 시점에 module import를 로드할 수 있도록 돕는다.**

import(”경로”) 형식으로 사용하며 Promise를 반환하며, export 하는 값들을 가진 객체를 반환한다. Promise를 반환하기 때문에 async await를 사용할 수 있다.

```jsx
import("module.js").then((module) => {
  const { default: Component, a, b } = module;
});

(async () => {
  await import("module.js");
  const { default: Component, a, b } = await import("module.js");
})();
```

```jsx
// 아래와 같은 방법도 가능하다.
if (true) {
  import("a.js");
}

import(`${id}.js`);

Promise.all([import("b.js"), import("c.js")]);
```

하지만 Dynamic import는 ES6문법이기 때문에 폴리필을 사용해야한다.

Webpack에서 Dynamic import를 사용하기 위해서는 `@babel/plugin-syntax-dynamic-import` 플러그인이 필요하다.

```jsx
// webpack.config.js
{
	loader: 'babel-loader',
	options: {
		plugins: ['@babel/plugin-syntax-dynamic-import']
	}
}
```

[@babel/plugin-syntax-dynamic-import · Babel](https://babeljs.io/docs/en/babel-plugin-syntax-dynamic-import?source=post_page-----caf62cc8c375--------------------------------)

Build 시점에 import() 모듈을 chunk 파일로 만들고, 필요한 시점에 header에 script를 세팅해 JS파일을 다운로드 한다.

```jsx
<script charset="utf-8" src="/static/js/2.chunk.js"></script>
```

이때 chunk 파일 명은 webpack의 output에 세팅한 파일 명을 따라간다.

```jsx
module.exports = {
  output: {
    filename: "[name].js",
    chunkFilename: "[name].chunk.js",
  },
};
```

만약 파일명을 지정하고 싶다면, 주석을 추가하여 파일명을 명시적으로 지정할 수 있다.

```jsx
import((/* webpackChunkName: "test" */ 'module.js')
  .then((module) => {
    const { default: Component, a, b } = module;
  });
```

### 3. Dynamic import 시점 (prefetch / preload)

Dynamic import에서 첫 페이지에는 필요하지 않지만 앞으로 필요할 만한 JS를 **미리 다운로드** 받아 둘 수 있다.

- `preload`: 프리로드는 **호출 시점에 브라우저가 반드시 가져오게** 한다. 브라우저가 현재 페이지에서 확실히 사용되는 곳에 이용한다.
- `prefetch`: 프리패치는 호출 시점에 가져오지 않고, **브라우저가 여유가 될 때 가져오게** 한다. 추후에 사용될 것 같은 곳에 이용한다.

두 기능은 모두 브라우저에게 언제 모듈을 불러올 것인 지 힌트를 준다. 이 기능은 `webpack 4`부터 사용할 수 있다.

```jsx
import(/* webpackPreload: true */ 'module.js');
import((/* webpackPrefetch: true */ 'module.js')
  .then((module) => {
    const { default: Component, a, b } = module;
  });
```

이때 **webpackPrefetch** 가 먼저 호출되었더라도 **webpackPreload** 가 먼저 처리된다.

이는 브라우저의 지원방식에 따라 다를 수 있다.

```jsx
<link rel="preload" as="script" href="module.js">
<link rel="prefetch" as="script" href="module.js">
```

기능이 아니라 호출 시점으로 제어 할 수도 있다.

### 4. React Dynamic import: Lazy와 Suspense

React를 사용한다면, Lazy를 이용해 Component가 사용 되는 시점에 가져오도록 구현할 수 있다.

```jsx
const Component = React.lazy(() => import("./Component"));

const Wrapper = () => {
  return (
    <div>
      <Component />
    </div>
  );
};
```

하지만 이 경우 아무것도 없는 페이지가 노출 되었다가 페이지가 전환된다.

Suspence를 이용하면 로딩중 같은 전환 화면을 구현할 수 있다.

이때 React.lazy는 여러 개여도 상관없다.

```jsx
const A = React.lazy(() => import("./A"));
const B = React.lazy(() => import("./B"));

const Wrapper = () => {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <A />
        <B />
      </Suspense>
    </div>
  );
};
```

만약 SPA로 페이지를 운영한다면, 이를 이용해 페이지 별로 파일을 나눌 수 있다.

```jsx
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";
import React, { Suspense, lazy } from "react";
const A = lazy(() => import("./page/a"));
const B = lazy(() => import("./page/b"));
const Page = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/a" component={A} />
        <Route path="/b" component={B} />
      </Switch>
    </Suspense>
  </Router>
);
```

## 2️⃣ 이미지 최적화

이미지는 웹사이트에서 중요한 시각적 요소이지만, **최적화되지 않은 이미지는 로딩 속도를 저하시켜 사용자 경험과 SEO에 악영향을 줄 수 있다.**

따라서 이미지 최적화 방법과 성능 향상을 위한 전략에 대해 알아보자.

### 1. 이미지 최적화의 중요성

- **빠른 로딩 속도**
  : 최적화된 이미지는 페이지 로딩 속도를 개선하여 사용자 이탈률을 줄이고, 전반적인 성능을 향상시킨다.
- **SEO 최적화**
  : Google은 페이지 속도를 중요한 순위 요소로 고려한다. 이미지 최적화는 검색 엔진 최적화(SEO)에도 긍정적인 영향을 준다.
- **적은 데이터 사용량**
  : 최적화된 이미지는 데이터 전송량을 줄여 모바일 사용자에게 더 나은 경험을 제공한다.

### 2. 이미지 최적화 방법

- **적절한 이미지 포맷 선택**
  : 이미지 포맷을 적절히 선택하면 품질을 유지하면서도 용량을 줄일 수 있다.
  - **JPEG**: 사진 및 그래디언트가 많은 이미지에 적합 (손실 압축)
  - **PNG**: 투명한 배경이 필요한 이미지에 적합 (무손실 압축)
  - **WebP**: JPEG보다 25~34% 더 높은 압축률 제공 (권장)
  - **AVIF**: WebP보다 더 높은 압축률과 품질을 제공 (최신 브라우저 지원 필요)
  - **SVG**: 아이콘, 로고 등 벡터 그래픽에 사용 (해상도 독립적)
- **이미지 크기 조정 및 압축
  :** 과도한 해상도의 이미지는 성능 저하를 유발할 수 있다. 따라서 사용 목적에 맞게 크기를 조정하고 압축해야 한다. - **TinyPNG** (https://tinypng.com/) - **Squoosh** (https://squoosh.app/) - **ImageOptim** (Mac 전용)
- **`srcset`과 `sizes` 속성을 활용한 반응형 이미지 적용**
  : 기기별 최적화된 이미지를 제공하면 불필요한 데이터 낭비를 줄일 수 있다.
      ```tsx
      <img
        srcset="image-480w.jpg 480w, image-800w.jpg 800w"
        sizes="(max-width: 600px) 480px, 800px"
        src="image-800w.jpg"
        alt="반응형 이미지">
      ```
- **`lazy loading`을 활용한 지연 로딩**
  : 스크롤 시점에 따라 이미지를 로딩하면 초기 페이지 로딩 속도를 개선할 수 있다.
      ```tsx
      <img src="image.jpg" loading="lazy" alt="지연 로딩 이미지">
      ```
- **이미지 CDN(Content Delivery Network) 사용
  :** 이미지 전용 CDN을 사용하면 사용자와 가까운 서버에서 이미지를 제공하여 로딩 속도를 높일 수 있다. - **Cloudinary** (https://cloudinary.com/) - **Imgix** (https://www.imgix.com/) - **Firebase Storage** (https://firebase.google.com/)

### 3. 이미지 최적화 체크리스트

✅ **적절한 이미지 포맷 선택 (WebP, AVIF 권장)**

✅ **해상도를 기기에 맞게 조정 (불필요한 고해상도 사용 지양)**

✅ **압축 도구를 활용하여 이미지 용량 최소화**

✅ **반응형 이미지(`srcset`, `sizes`) 적용**

✅ **지연 로딩(`lazy loading`) 활성화**

✅ **이미지 CDN 사용으로 로딩 속도 개선**

## 3️⃣ 폰트 최적화

웹사이트의 타이포그래피는 사용자 경험과 디자인에서 중요한 요소이다.

하지만 **웹 폰트(font)를 잘못 로딩하면 성능 저하, 레이아웃 시프트, 깜빡이는 텍스트(FOUT, FOIT) 등의 문제가 발생할 수 있다.**

따라서 웹 폰트 로딩 전략과 최적화 기법을 알아보자.

### 1. 웹 폰트 로딩의 문제점

웹 폰트 사용 시 발생할 수 있는 주요 문제점은 다음과 같다.

- **FOUT (Flash of Unstyled Text)**: 기본 폰트로 먼저 표시된 후 웹 폰트가 적용되는 현상
- **FOIT (Flash of Invisible Text)**: 웹 폰트가 로드될 때까지 텍스트가 보이지 않는 현상
- **레이아웃 시프트**: 웹 폰트가 적용되면서 텍스트 크기가 변경되어 레이아웃이 흔들리는 문제
- **성능 저하**: 불필요한 폰트 로딩으로 인해 페이지 속도가 느려짐

이러한 문제를 해결하기 위해 적절한 로딩 전략과 최적화 기법을 적용해야 한다.

### 2. 웹 폰트 로딩 전략

1.  **`font-display` 속성 활용**

CSS의 `font-display` 속성을 활용하면 웹 폰트 로딩 방식을 조절할 수 있다.

```jsx
@font-face {
  font-family: 'CustomFont';
  src: url('/fonts/custom-font.woff2') format('woff2');
  font-display: swap;
}
```

- `auto`: 브라우저 기본 동작을 따름
- `swap`: 웹 폰트가 로드될 때까지 시스템 폰트 사용 후 교체 (FOUT 발생)
- `block`: 웹 폰트 로딩 전까지 텍스트 숨김 (FOIT 발생 가능)
- `fallback`: 일정 시간 동안 웹 폰트가 로드되지 않으면 대체 폰트 사용
- `optional`: 폰트 로드 시간이 너무 길어지면 웹 폰트 적용을 포기

일반적으로 `swap`을 권장하며, 필요에 따라 `fallback`이나 `optional`을 고려할 수 있다.

1. **웹 폰트 서브셋팅 (Subsetting)**

사용하지 않는 글리프(glyph)를 제거하여 폰트 파일 크기를 줄이는 기법이다.
Google Fonts에서는 서브셋 옵션을 제공하고 있고, 직접 최적화하려면 다음과 같은 도구를 사용할 수도 있다.

- [glyphhanger](https://github.com/filamentgroup/glyphhanger)
- [Font Squirrel](https://www.fontsquirrel.com/)

예를 들어, 한글 폰트에서 자주 사용하는 글자만 남기고 서브셋팅하면 파일 크기를 크게 줄일 수 있습니다.

1. **폰트 포맷 최적화**

웹 폰트는 여러 포맷이 있지만, 최신 브라우저에서는 `WOFF2`가 가장 효율적이다.

- **WOFF2**: 가장 압축률이 높고 성능이 좋음 (권장)
- **WOFF**: 구형 브라우저 지원 필요 시 사용
- **TTF, OTF**: 웹에서 사용하기엔 비효율적 (가능하면 변환)

1. **폰트 로딩을 비동기 처리**

웹 폰트 로딩을 비동기 처리하면 페이지 로딩 속도를 개선할 수 있다.
Google Fonts를 사용할 경우 `display=swap` 옵션과 함께 `preconnect`를 추가하면 성능을 향상할 수 있다.

```jsx
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
```

1. **`preload`를 이용한 폰트 로딩 최적화**

웹 폰트를 미리 로드하면 텍스트 렌더링 속도를 높일 수 있다.

```jsx
<link rel="preload" href="/fonts/custom-font.woff2" as="font" type="font/woff2" crossorigin="anonymous">
```

`preload`를 사용하면 브라우저가 폰트를 미리 로드하지만, 사용하지 않는 폰트를 `preload`하면 오히려 성능이 저하될 수 있으므로 주의해야 한다.

### 웹 폰트 최적화 체크리스트

✅ **필요한 글자만 포함된 서브셋 폰트 사용**

✅ **최적화된 포맷(WOFF2) 사용**

✅ **`font-display: swap` 적용**

✅ **Google Fonts 사용 시 `display=swap` 및 `preconnect` 추가**

✅ **자주 사용하는 폰트는 `preload` 적용**

✅ **비동기 로딩을 활용하여 렌더링 블로킹 방지**
