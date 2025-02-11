
# 설정(개발환경)

### **📌Webpack이란?**

**모듈 번들러**

여러 개의 자바스크립트 파일을 하나로 묶어 번들링 ⇒ **브라우저에서 효율적으로 로드 가능**

- **코드 스플리팅**: 필요한 부분만 로드할 수 있도록 번들 파일 분리
- **트리 셰이킹**: 사용되지 않는 코드를 제거
- **로더**: CSS, 이미지 파일도 변환 (웹팩은 모든 파일을 모듈로 바라봄)
- **플러그인**: 번들 결과물에 대한 추가 기능(처리)

**기본 설정 (`webpack.config.js`)**

```

module.exports = {
  entry: "./src/index.js", // 번들링 시작 파일
  output: {
    filename: "bundle.js", // 번들 결과물 파일명
    path: path.resolve(__dirname, "dist"), // 결과물 저장 경로
  },
  module: {
    rules: [
      {
        test: /\.js$/, // .js 파일을 대상
        exclude: /node_modules/, // node_modules 제외
        use: {
          loader: "babel-loader", // Babel을 사용하여 변환
        },
      },
    ],
  },
  mode: "development", // development 또는 production
};

```

### **📌Babel이란?**

**최신 JavaScript 코드를 구형 브라우저에서도 동작하도록 변환해주는 JavaScript 컴파일러**

- 최신 ES6+ 문법을 ES5로 변환
- Polyfill을 적용하여 최신 API 지원

⇒ 보통 웹팩 설정에서 바벨로더로 사용

### **📌 Polyfill이란?**

**브라우저가 지원하지 않는 최신 JavaScript 기능을 사용할 수 있도록 구현된 코드 조각**

```json
json
복사편집
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "useBuiltIns": "entry",
        "corejs": 3 //폴리필 사용
      }
    ]
  ]
}
```

---

### **📌ESLint란?**

**JavaScript 코드의 문법과 스타일을 검사하고 오류를 찾아주는 정적 코드 분석 도구**

- 일관된 코드 스타일 유지
- 잠재적인 버그 발견

**기본 설정**

```json

{
  "env": {
    "browser": true,
    "es2021": true},
  "extends": "eslint:recommended",
  "rules": {
    "no-unused-vars": "warn",
    ...
  }
}
```

---

### **📌 Prettier란?**

**코드 스타일을 자동으로 정리해주는 코드 포매터**

- 일관된 코드 포맷 유지 (자동 줄 바꿈, 들여쓰기, 세미콜론 추가 등)
- 코드 가독성 향상
- 협업 - 코드 스타일 충돌 방지

**기본 설정**

```json

{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2
  ...
}

```

### **📌 Private 속성이란?**

프로젝트를 **npm에 배포할 수 없도록 방지하는 기능**

```json

{
  "name": "my-project",
  "version": "1.0.0",
  "private": true
}

```

private: true 속성 안 해놓으면, 실수로 `npm publish`를 실행했을 때 패키지가 배포될 수 있음.

---

### **📌 dependencies**

- 실제 프로젝트 실행에 필요한 라이브러리
    
    ex) React, Express, Axios 등
    
- 배포 후에도 포함됨

```json
{
  "dependencies": {
    "react": "^18.0.0",
    "axios": "^1.0.0"
  }
}
```

### **📌 devDependencies**

- 개발할 때만 필요한 도구 (빌드, 테스트, 린트 등)
    
    ex) Webpack, ESLint, Babel, Jest 
    
- 배포 시 포함되지 않음

```json
{
  "devDependencies": {
    "webpack": "^5.0.0",
    "eslint": "^8.0.0"
  }
}
```

---