# 환경설정

## 📌 Notion 문서 링크
[📖 Notion 문서 바로가기](https://bokjunwoo.notion.site/19577d2ca3638007982bd78ed16a3ee8)

## 🔎 환경설정
# Webpack & Babel & **Polyfill**

## Webpack이란? (모듈 번들러)

**Webpack**은 **HTML, CSS, JavaScript, 이미지 등 다양한 파일을 하나의 번들 파일로 묶어주는 도구**입니다.
웹 애플리케이션을 개발할 때 JavaScript 코드가 여러 개의 파일로 나뉘어 있을 경우, **Webpack이 이를 하나의 파일로 번들링하여 성능을 최적화**합니다.

### **Webpack의 역할**

1. **모듈 번들링**
    - 여러 개의 파일을 하나의 번들 파일로 묶음.
    - 예: `index.js`, `style.css`, `app.js` → `bundle.js` 하나로 결합.
2. **모듈 시스템 지원**
    - ES6+의 `import/export` 문법을 사용하여 모듈을 관리 가능.
    - CommonJS(`require()`) 등의 다양한 모듈 방식을 통합.
3. **트리 쉐이킹(Tree Shaking)**
    - 사용되지 않는 코드를 제거하여 번들 크기를 줄임.
4. **코드 스플리팅(Code Splitting)**
    - 페이지별로 필요한 코드만 로드하여 **초기 로딩 속도 향상**.
5. **로더(Loader) 지원**
    - CSS, 이미지, 폰트 등도 JavaScript 파일에서 `import` 가능.

## Babel이란? (JavaScript 컴파일러)

**Babel**은 **최신 JavaScript(ES6~ESNext)를 구형 브라우저에서도 실행 가능하도록 변환하는 JavaScript 컴파일러** 입니다. 즉, 브라우저가 지원하지 않는 최신 문법을 **구형 브라우저에서도 동작할 수 있도록 변환**해 줍니다.

### **Babel이 필요한 이유**

1. **최신 문법을 구형 브라우저에서도 실행 가능하도록 변환**
    - 예) `const`, `let`, 화살표 함수(➡️), `async/await` 등 최신 기능을 지원하지 않는 브라우저(IE11 등)를 위해 변환.
2. **ES6+ 문법 변환 (ES6 → ES5)**
    
    ```jsx
    const sum = (a, b) => a + b;
    
    // Babel을 사용하면 아래와 같이 변환됩니다.
    "use strict";
    function sum(a, b) {
      return a + b;
    }
    ```
    
3. **React JSX 문법 변환**
    - JSX(`<Component />`)를 일반 JavaScript 코드로 변환 가능.

## **Polyfill**이란? (브라우저 기능 추가)

**Polyfill**은 **구형 브라우저에서 지원되지 않는 JavaScript 기능을 추가하는 코드**입니다.
Babel이 문법을 변환하는 역할이라면, **Polyfill은 기능을 추가하는 역할**을 합니다.

### **Polyfill이 필요한 이유**

구형 브라우저는 최신 JavaScript API를 지원하지 않습니다.

```jsx
[1, 2, 3].at(-1);

Promise.resolve(1);

new Set(1, 2, 3);
```

최신 브라우저에서는 잘 동작하지만, 오래된 브라우저에서는 실패합니다. 
이런 문제를 해결하기 위해서는 오래된 브라우저에서 없는 구현을 채워주어야 합니다. 이렇게 구현을 채워주는 스크립트를 **Polyfill**이라고 합니다.

### **Polyfill의 문제**

위와 같이 코드를 작성하면 폭넓은 브라우저를 지원할 수 있다는 장점이 있지만 문제가 하나 생깁니다. 불러와야 하는 JavaScript 코드가 많아진다는 점입니다. 실행해야 하는 Polyfill 스크립트가 많아질수록 사용자가 경험하는 웹 서비스의 성능은 나빠집니다.

# ESLint와 Prettier

웹 개발에서는 **코드 스타일 유지, 문법 오류 방지, 일관성 있는 코드 작성**이 중요합니다.

이를 위해 사용되는 대표적인 도구가 **ESLint와 Prettier**입니다.

- **ESLint** → **JavaScript 코드에서 오류를 찾아주는 정적 분석 도구**
- **Prettier** → **코드 스타일을 자동으로 포맷팅하는 도구**

## ESLint란?

**ESLint**는 **JavaScript 코드에서 문법 오류, 버그, 비효율적인 코드 패턴을 찾아주는 정적 분석 도구** 입니다.
Linting(린팅)이란**코드를 분석하여 오류를 찾아내는 과정**을 의미합니다.

### **ESLint가 필요한 이유**

1. **코드에서 문법 오류를 사전에 방지**
    - `console.log(a);` → `a`가 선언되지 않았으면 오류 감지.
2. **일관된 코드 스타일 유지**
    - `const` vs `let` 사용 규칙 적용 가능.
3. **불필요한 코드 감지**
    - 사용하지 않는 변수, 중복 코드 등을 자동으로 경고.
4. **팀 프로젝트에서 코드 스타일 통일**
    - 개발자마다 스타일이 다르면 유지보수가 어려움 → **ESLint가 스타일을 통일**.

### **ESLint 설정 파일 (`.eslintrc.json`)**

```json
{
  "env": {
    "browser": true,
    "es2021": true},
  "extends": ["eslint:recommended", "plugin:react/recommended"],
  "rules": {
    "no-unused-vars": "warn", // 사용하지 않는 변수 경고
    "eqeqeq": "error", // === 사용 강제
    "quotes": ["error", "single"], // 문자열은 ' ' 사용
    "semi": ["error", "always"] // 세미콜론 강제 사용
  }
}

```

## **Prettier란?**

### **Prettier의 개념**

**Prettier**는 **코드 스타일을 자동으로 포맷팅해주는 도구**입니다.

ESLint는 **코드의 오류를 검사**, Prettier는 **코드의 스타일을 통일**하는 역할을 합니다.

### **Prettier가 필요한 이유**

1. **자동으로 코드 스타일 통일**
    - 탭/스페이스, 따옴표, 세미콜론 등을 자동 정리.
2. **팀 프로젝트에서 코드 일관성 유지**
    - 코드 리뷰 시 스타일 문제로 시간 낭비 방지.
3. **설정만 해두면 저장할 때 자동 포맷팅**
    - `prettier --write` 실행 시 코드 스타일 자동 정리.

### **Prettier 설정 파일 (`.prettierrc.json`)**

```json
{
  "printWidth": 80, // 한 줄에 최대 80자
  "tabWidth": 2, // 탭 크기 2칸
  "singleQuote": true, // 따옴표는 ' ' 사용
  "semi": true, // 세미콜론 사용
  "trailingComma": "es5" // 마지막 요소 콤마 허용
}
```

## **ESLint vs. Prettier 차이점**

| **기능** | **ESLint** | **Prettier** |
| --- | --- | --- |
| 코드 오류 감지 | ✅ 있음 | ❌ 없음 |
| 코드 스타일 적용 | 일부 가능 | ✅ 자동 적용 |
| 세미콜론 제거/추가 | ❌ 없음 | ✅ 있음 |
| 코드 자동 정리 | 일부 가능 (`--fix`) | ✅ 있음 (`--write`) |
| 추천 사용 방식 | 코드 문법 검사 | 코드 스타일 정리 |

# package.json

## **`package.json`이란?**

`package.json`은 **프로젝트의 기본 정보, 의존성 라이브러리, 실행 스크립트, 설정 등을 JSON 형식으로 관리**하는 파일입니다.

### **`package.json`의 주요 역할**

| 역할 | 설명 |
| --- | --- |
| **프로젝트 정보 관리** | 프로젝트 이름, 버전, 설명, 라이센스 정보 포함 |
| **의존성(Dependencies) 관리** | npm 패키지를 설치하고, 버전을 관리 |
| **스크립트(Scripts) 관리** | `npm start`, `npm test` 등의 명령어를 정의 |
| **배포 설정** | `private: true`를 설정하면 배포 방지 |
| **호환성 관리** | 지원하는 Node.js 버전 지정 가능 |

## **`private` 속성이란?**

- `"private": true`를 설정하면 **npm에 실수로 배포하는 것을 방지**할 수 있습니다.
- 일반적으로 **개인 프로젝트, 내부 라이브러리, 기업 내부 패키지**에서 사용됩니다.

### **1) `private: true`가 설정된 경우**

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "private": true
}
```

- `npm publish` 실행 시, **배포가 차단됨**
- 터미널 오류 메시지
    
    ```
    npm ERR! This package has been marked as private
    npm ERR! Remove the 'private' field to publish it.
    ```
    
- **목적**: 실수로 공개 패키지로 업로드하는 것을 방지.

### **2) `private: false` 또는 속성 없음**

```json
{
  "name": "my-library",
  "version": "1.0.0"
}
```

- `npm publish` 실행 시, **정상적으로 배포 가능**
- 이 경우, 실수로 `npm publish` 하면 **npm 공개 저장소(npm registry)에 업로드됨**.

## **`private` 속성을 사용하는 이유**

| 사용 사례 | 이유 |
| --- | --- |
| **개인 프로젝트** | npm에 배포할 필요가 없으므로 실수 방지 |
| **기업 내부 프로젝트** | 사내에서만 사용하므로 npm 배포를 원하지 않음 |
| **Monorepo(모노레포) 프로젝트** | 내부 패키지는 개별 배포하지 않고 루트에서 관리 |

## `dependencies` vs `devDependencies` 차이

### **`dependencies`란? (운영 환경에서 필요한 패키지)**

`dependencies`는 **프로덕션(운영 환경)에서 반드시 필요한 패키지**입니다.

즉, 애플리케이션이 실행될 때 **필수적으로 필요한 모듈**을 포함합니다.

### **`devDependencies`란? (개발 환경에서만 필요한 패키지)**

`devDependencies`는 **개발할 때만 필요한 패키지**로, 프로덕션에서는 사용되지 않습니다.

즉, **빌드, 테스트, 코드 스타일 체크** 등의 도구들이 여기에 포함됩니다.

### **`dependencies` vs `devDependencies` 차이 정리**

| 비교 항목 | `dependencies` | `devDependencies` |
| --- | --- | --- |
| **설치 명령어** | `npm install 패키지명` | `npm install 패키지명 --save-dev` |
| **사용 환경** | 운영(프로덕션) 환경 | 개발 환경(로컬, 테스트) |
| **배포 시 포함 여부** | ✅ 포함됨 | ❌ 포함되지 않음 |
| **예제 패키지** | `express`, `mongoose`, `axios` | `eslint`, `jest`, `webpack` |
| **설치 시 기본 포함 여부** | `npm install` 실행 시 포함 | `npm install --production` 실행 시 제외 |