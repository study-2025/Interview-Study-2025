# 렌더링방식

## 📌 Notion 문서 링크
[📖 Notion 문서 바로가기](https://bokjunwoo.notion.site/19577d2ca36380238f51cbfe139e6e64)

## 🔎 미리보기
# **SPA vs MPA 완벽 정리**

웹 애플리케이션을 개발할 때 한 번의 HTML 로드 후 동적으로 페이지를 변경하는 방식(SPA)과 **페이지 이동 시마다 새로운 HTML을 받아오는 방식(MPA)** 중에서 선택해야 합니다.

## **SPA (Single Page Application, 단일 페이지 애플리케이션)**

### **SPA란?**

- 웹 애플리케이션이 **최초에 한 번만 HTML을 로드한 후, 이후 페이지 전환은 JavaScript로 처리**하는 방식.
- 새로운 페이지를 요청하지 않고, **클라이언트에서 기존 HTML을 변경하여 동작**.
- **React, Vue, Angular** 같은 프론트엔드 프레임워크에서 사용.

### **SPA 동작 방식**

1. 사용자가 웹사이트에 접속 → 서버에서 **최초 HTML + JavaScript + CSS를 로드**.
2. 이후 페이지 이동 시 **브라우저가 새로운 HTML을 요청하지 않고, JavaScript가 동적으로 내용 변경**.
3. API 요청을 통해 필요한 데이터를 가져와 화면을 업데이트.

**SPA 예제 (React)**

```tsx
import { useState } from "react";

function App() {
  const [page, setPage] = useState("home");

  return (
    <div>
      <nav>
        <button onClick={() => setPage("home")}>Home</button>
        <button onClick={() => setPage("about")}>About</button>
      </nav>
      {page === "home" && <h1>Home Page</h1>}
      {page === "about" && <h1>About Page</h1>}
    </div>
  );
}
export default App;

```

**특징**

- 페이지 이동 시 **서버에서 새로운 HTML을 요청하지 않음**.
- JavaScript가 동적으로 **DOM을 변경하여 렌더링**.

### **SPA의 장점 & 단점**

| 장점 | 단점 |
| --- | --- |
| **빠른 페이지 전환 (No Reload)** | **초기 로딩 속도 느림** (모든 JS 파일을 한 번에 받음) |
| 서버 요청 최소화 → **부하 감소** | **SEO(검색엔진 최적화)가 어려움** |
| **부드러운 UI/UX (앱 같은 경험)** | **JavaScript 의존도가 높음** (JS 비활성화 시 작동 불가) |
| **프론트엔드 개발 생산성 높음** | **메모리 사용량 증가 (싱글 페이지 유지)** |

## **MPA (Multi Page Application, 다중 페이지 애플리케이션)**

### **MPA란?**

- 사용자가 새로운 페이지로 이동할 때마다 **서버에서 새로운 HTML을 요청하고, 전체 페이지를 새로고침하는 방식**.
- 과거의 전통적인 웹사이트에서 사용되며, **SEO 최적화에 유리**.

### **MPA 동작 방식**

1. 사용자가 웹사이트에 접속 → 서버에서 **HTML 파일을 로드**.
2. 새로운 페이지로 이동할 때 **서버에서 새로운 HTML을 요청 → 전체 페이지 새로고침**.
3. 서버에서 **완전한 HTML을 제공**하여 검색 엔진이 쉽게 인식 가능.

**특징**

- 페이지 전환 시 **새로운 HTML을 요청하고 새로고침 발생**.
- 검색 엔진(SEO)에 **최적화된 구조**

### **MPA의 장점 & 단점**

| 장점 | 단점 |
| --- | --- |
| **SEO(검색 엔진 최적화) 가능** | **페이지 이동 시 새로고침 발생 → 속도 저하** |
| **초기 로딩 속도가 빠름** | 서버 요청이 많아져 **트래픽 증가** |
| **JavaScript 없이도 작동 가능** | **프론트엔드 개발 복잡도 증가** (서버와의 강한 결합) |
| **보안성이 높음** (각 페이지 별 처리) | 유지보수 어려움 (코드 중복 가능성 증가) |

## **SPA + MPA 혼합 사용 (Next.js)**

SPA와 MPA의 장점을 조합하면 **빠른 로딩 속도 + SEO 최적화**를 모두 잡을 수 있습니다.

예를 들어, Next.js의 **SSR(서버 사이드 렌더링) 또는 SSG(정적 사이트 생성)** 방식을 사용하면 **SEO 최적화가 필요한 MPA 기능을 유지하면서도 SPA의 속도를 활용 가능**.

# CSR & SSR & SSG & ISG

## **CSR (Client-Side Rendering, 클라이언트 사이드 렌더링)**

### **CSR이란?**

- **클라이언트(브라우저)에서 JavaScript로 HTML을 동적으로 생성하는 방식**.
- 최초 요청 시 **빈 HTML 파일을 받고, 이후 JavaScript가 데이터를 가져와 화면을 구성**.

### **CSR의 흐름**

1. **사용자가 웹사이트 접속**
2. **서버에서 빈 HTML + JavaScript를 반환**
3. **브라우저가 JavaScript 실행 → API 요청**
4. **응답 받은 데이터로 동적 렌더링**

### **CSR 예제**

**React의 `useEffect()`를 이용한 API 요청 후 렌더링**

```jsx
import { useEffect, useState } from "react";

function CSRExample() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch("https://api.example.com/data")
      .then(res => res.json())
      .then(data => setData(data));
  }, []);

  if (!data) return <p>Loading...</p>;
  return <div>{data.message}</div>;
}

export default CSRExample;
```

### **CSR의 장점 & 단점**

| 장점 | 단점 |
| --- | --- |
| 서버 부담이 적음 (트래픽 감소) | 초기 로딩 속도가 느림 |
| 빠른 인터랙션 가능 (SPA) | SEO(검색엔진 최적화)에 불리함 |
| 개발이 간단하고 유연함 | JavaScript 비활성화 환경에서는 동작 안 함 |
- **CSR은 SPA(Single Page Application) 개발에 적합**.

## **SSR (Server-Side Rendering, 서버 사이드 렌더링)**

### **SSR이란?**

- **서버에서 HTML을 완성한 후 클라이언트에게 반환하는 방식**.
- 브라우저에서 JavaScript가 실행되기 전에도 **완전한 HTML을 받기 때문에 SEO 최적화가 가능**.

### **SSR의 흐름**

1. **사용자가 웹사이트 접속**
2. **서버에서 API 요청 후, HTML을 미리 생성**
3. **완성된 HTML을 클라이언트에 전달**
4. **브라우저가 페이지를 표시**

### **SSR 예제 (Next.js)**

```tsx
import { GetServerSideProps } from "next";

export default function SSRExample({ data }) {
  return <div>{data.message}</div>;
}

export const getServerSideProps: GetServerSideProps = async () => {
  const res = await fetch("https://api.example.com/data");
  const data = await res.json();

  return { props: { data } };
};

// app router
export default async function SSRPage() {
  const res = await fetch("https://api.example.com/data", { cache: "no-store" });
  const data = await res.json();

  return <div>{data.message}</div>;
}
```

### **SSR의 장점 & 단점**

| 장점 | 단점 |
| --- | --- |
| SEO 최적화 가능 | 서버 부하 증가 (모든 요청 처리) |
| 첫 페이지 로딩 속도가 빠름 | 클라이언트와 서버 간 네트워크 비용 발생 |
| 최신 데이터를 즉시 반영 가능 | 서버가 다운되면 페이지 접근 불가 |
- **SSR은 SEO가 중요한 웹사이트에 적합** (ex: 블로그, 뉴스 사이트).

## **SSG (Static Site Generation, 정적 사이트 생성)**

### **SSG란?**

- **빌드 타임(배포 시점)에 HTML을 미리 생성하여 정적 파일로 제공하는 방식**.
- **모든 페이지가 미리 만들어져 있어, 요청 시 서버에서 바로 응답할 수 있음** → 매우 빠름.

### **SSG의 흐름**

1. **배포 전에 HTML을 미리 생성** (`npm run build`)
2. **서버에 정적 파일(HTML, CSS, JS) 배포**
3. **사용자가 접속하면 미리 만들어진 HTML을 즉시 반환**
4. **필요하면 클라이언트에서 추가 데이터를 요청하여 업데이트 가능**

### **SSG 예제 (Next.js)**

```tsx
import { GetStaticProps } from "next";

export default function SSGExample({ data }) {
  return <div>{data.message}</div>;
}

export const getStaticProps: GetStaticProps = async () => {
  const res = await fetch("https://api.example.com/data");
  const data = await res.json();

  return { props: { data } };
};

/app router
fetch()에서 cache: "force-cache"를 사용하여 정적 HTML 생성

export default async function SSGPage() {
  const res = await fetch("https://api.example.com/data", { cache: "force-cache" });
  const data = await res.json();

  return <div>{data.message}</div>;
}
```

### **SSG의 장점 & 단점**

| 장점 | 단점 |
| --- | --- |
| 페이지 로딩 속도가 매우 빠름 | 데이터가 자주 변경되는 사이트에 적합하지 않음 |
| 서버 부하가 거의 없음 | 빌드 타임이 길어질 수 있음 |
| SEO 최적화 가능 | 새 데이터를 반영하려면 다시 배포해야 함 |
- **SSG는 데이터가 자주 변경되지 않는 사이트에 적합** (ex: 블로그, 문서 사이트, 랜딩 페이지).

## **ISR (Incremental Static Regeneration, 점진적 정적 생성)**

### **ISR란?**

- **SSG의 단점(새로운 데이터 반영 불가능)을 해결하기 위해 도입된 방식**.
- **일부 페이지를 특정 시간마다 다시 생성하여 최신 데이터를 반영 가능**.
- **정적 페이지의 빠른 속도 + SSR의 동적 데이터 반영 기능을 조합한 방식**.

### **ISR의 흐름**

1. **배포 시 정적 HTML을 생성**
2. **사용자가 요청하면 기존 HTML을 제공**
3. **특정 시간(`revalidate`)이 지나면 다시 HTML을 생성하여 최신 데이터 반영**

### **ISR 예제 (Next.js)**

```tsx
import { GetStaticProps } from "next";

export default function ISGExample({ data }) {
  return <div>{data.message}</div>;
}

export const getStaticProps: GetStaticProps = async () => {
  const res = await fetch("https://api.example.com/data");
  const data = await res.json();

  return { props: { data }, revalidate: 10 }; // 10초마다 페이지를 재생성
};

// app router
fetch()에서 next: { revalidate: 시간 }을 사용하여 일정 주기로 새 데이터 반영.

export default async function ISGPage() {
  const res = await fetch("https://api.example.com/data", { next: { revalidate: 10 } });
  const data = await res.json();

  return <div>{data.message}</div>;
}
```

### **ISR의 장점 & 단점**

| 장점 | 단점 |
| --- | --- |
| SSG처럼 빠르면서도 최신 데이터 반영 가능 | 즉각적인 데이터 반영이 어려울 수 있음 |
| 서버 부하를 줄이면서도 동적 컨텐츠 제공 가능 | 캐싱 문제 발생 가능 |
- **ISR는 블로그, 뉴스 사이트, 동적 컨텐츠가 있는 페이지에 적합**.

## **CSR, SSR, SSG, ISR 비교 정리**

| 방식 | 로딩 속도 | SEO | 최신 데이터 반영 | 서버 부하 |
| --- | --- | --- | --- | --- |
| **CSR** | ❌ 느림 | ❌ 없음 | ✅ 즉시 반영 | ❌ 없음 |
| **SSR** | ⏳ 보통 | ✅ 가능 | ✅ 즉시 반영 | ❌ 높음 |
| **SSG** | 🚀 빠름 | ✅ 가능 | ❌ 배포해야 반영 | ✅ 없음 |
| **ISR** | 🚀 빠름 | ✅ 가능 | ✅ 일정 시간마다 반영 | ✅ 낮음 |

**CSR** → SPA, 빠른 인터랙션 필요할 때 (ex: 대시보드, 관리자 페이지)

**SSR** → SEO가 중요한 동적 페이지 (ex: 뉴스, 실시간 데이터)

**SSG** → 변경이 적은 정적 콘텐츠 (ex: 블로그, 문서)

**ISR** → 정적 페이지이지만 일정 시간마다 업데이트가 필요한 경우 (ex: 뉴스, 가격 변동 페이지)