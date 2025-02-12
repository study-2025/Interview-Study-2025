## 타입스크립트 탄생 배경

![[Pasted image 20250212162619.png]]
[https://serokell.io/blog/why-typescript](https://serokell.io/blog/why-typescript)

**`타입스크립트`**는 **`자바스크립트`**에서 파생된 이름이며,

**`자바스크립트`** 에서 **타입 정의와 체킹이 추가된 슈퍼셋(Superset: 상위 집합)이라고 할 수 있다.**

이러한 **`타입스크립트`**는 왜 탄생하게 되었을까?

### 에러의 사전 방지 (feat. 동적 타입 언어 JS)

**`자바스크립트`**는 **동적 타입 언어**로, 개발할 때 변수가 어떤 타입인 지 명시하지 않는다.

따라서 개발을 하다 보면 이와 같은 특징 때문에 의도하지 않은 오류가 발생할 수도 있다.

```jsx
function add(a, b) {
  return a + b;
}

const a = "1";
const b = 2;

console.log(add(a, b)); // 12
```

예를 들어 위와 같이 a와 b를 더하는 함수를 작성했다고 했을 때, 위 코드는 에러 없이 동작하는 매우 정상적인 코드이지만, 개발자의 의도는 이것이 아닐 것이다.

이처럼 오류 없이 코드가 실행되지만 의도한 동작이 아닌 경우도 있고, 아예 코드가 실행되지 않아 빈 화면이 뜨거나 멈춰 버리는 경우도 있다.

즉 **`자바스크립트**`**는 **오류의 발생 여부를 런타임에서만 확인할 수 있다는 단점이 있다.

따라서 **`타입스크립트`**는 이러한 자바스크립트의 단점을 보완하기 위해

- **변수의 타입을 명시**하고
- **정적 분석**을 통해
- 런타임에 발생할 수 있는 에러를 미리 해결할 수 있도록 한다.

**`타입스크립트`**를 사용하면,

아래와 같이 IDE에서 빨간 줄로 힌트를 주는 경우도 있고,

**컴파일 타임에 에러가 발생**하여 미리 검출 및 해결할 수 있다.

![[Pasted image 20250212162647.png]]
[https://www.youtube.com/watch?v=9K4EL1jeSmk](https://www.youtube.com/watch?v=9K4EL1jeSmk)

만약 **`타입스크립트`**처럼 미리 발생할 에러를 잡아주지 않으면 실제 프로덕션 환경에 배포되어 사용자들이 피해를 본 다음 고칠 수 밖에 없을 것이다.

<aside> 💡 자바스크립트는 런타임(실행)에 오류를 검출하고 타입스크립트는 컴파일 중에 오류를 검출한다.

</aside>

## 타입스크립트의 오해

### 1. 브라우저 호환성과 빌드 시간

우리가 사용하는 크롬, 사파리 등의 **브라우저**는 **`자바스크립트`** 엔진 만을 내장하고 있기 때문에 **`타입스크립트`**라는 언어를 이해하지 못한다.

따라서 **`타입스크립트`**로 작성된 언어는 프로덕션에 올라가기 전에 반드시 **`자바스크립트`** 파일로 변환을 거쳐야 한다. 이렇게 변환하는 작업을 **`트랜스파일`**이라고 한다.

**`컴파일`**이 **고급 언어를 저급 언어로 변환**해주는 것을 뜻한다면,

**`트랜스파일`**은 **비슷한, 동등한 수준의 언어로 변환하는 것**을 말한다.

따라서 브라우저는**`타입스크립트`**를 이해할 수 없기 때문에

**`자바스크립트`**로 **`트랜스파일`**하여 실제 브라우저에서 실행하게 된다.

그러면 괜히 타입스크립트를 쓰면

1. 트랜스파일을 해야하고
2. 빌드시간이 늘어나고

더 비효율적인 게 아니냐고 할 수도 있다.

이는 합리적인 추론이며 실제로도 타입 체킹 때문에 빌드 시간이 조금 더 늘어나기도 한다.

하지만 현실에서는, 프론트엔드 코드는 개발사가 세팅해둔 서버에서 돌아가는 것이 아니라 사용자의 디바이스에서 돌아간다.

이때 극단적으로 사용자의 디바이스가 더 이상 업데이트가 진행되지 않는 갤럭시 S2일수도 있다.

따라서 이러한 디바이스도 지원할 수 있도록, 즉 구형 브라우저(구형 자바스크립트 엔진을 가진)에서도 잘 실행될 수 있도록 **`폴리필`**을 사용하여 최신 JS를 구형 JS로 변환해준다.

즉 **프론트엔드 코드는 거의 항상 `폴리필`을 사용해서 구형 브라우저에서도 잘 돌아갈 수 있도록 코드를 변환하는 작업**을 거친다.

따라서 이 과정과 함께 TS를 JS로 변환하면 되기 때문에 **TS 트랜스파일 과정이 더해진다고 해서 크게 작업상 문제가 되지는 않는다.**

이왕 코드 작업 마치고 브라우저 호환성과 최적화를 위해 변환 작업을 거친다면, 개발할 때 안전성과 편리성을 위해 TS를 사용하고 같이 변환하자는 것이다.

### 2. 타입 선언하느라 생산성이 더 저해되는 것 아닌지?

정적 타입 언어를 처음 접하면, 비즈니스 로직만 작성하면 되지 변수 타입까지 선언해주어야 하나,

코딩하는 시간이 더 늘어나는 것이 아닌가 생각할 수 있다.

이는 사람마다 생각의 차이가 있지만, 타입을 선언함으로써 얻는 장점도 크기 때문에 무시할 수 없다.

사람은 완벽하지 않기 때문에 필연적으로 실수를 할 수 밖에 없다.

이처럼 사람이 실수를 하는 것을 **휴먼 에러**라고 한다.

만약 1000줄의 코드를 작성했는데, 웹 페이지를 띄웠을 때 의도와 다르게 빈 화면이 뜬다면, 또 그 에러가 타입 때문에 발생한 문제라면 쉽게 에러를 발생 시키는 코드를 찾아낼 수 있을까?

이처럼 자바스크립트만 사용할 땐 실제 페이지를 띄웠을 때 의도와 다르게 빈 화면이 떠야지만 오류를 검출할 수 있고, 또 어느 부분에서 오류가 발생하는 지 일일이 찾아주어야 한다.

반면 타입스크립트를 사용하면 IDE에서 개발 중에 바로바로 에러를 표시해주기 때문에 생산성이 더 높아질 수 있다.

그리고 타입스크립트에서는 객체의 속성 타입도 미리 선언해주기 때문에, IDE에서 특정 객체가 어떤 속성을 가지고 있는 지 미리 알 수 있고 속성을 타이핑할 때 자동 완성이 되어 작업 속도가 더 빨라진다는 의견도 있다.

### 3. 에버 그린 브라우저

최근에는 최신 브라우저의 발전 덕에, 브라우저 스스로 업데이트를 하면서 최신 상태를 유지하는 기능을 가지는데, 이런 브라우저를 에버 그린 브라우저라고 한다.

이런 브라우저가 많아지면 더 이상 개발자가 브라우저 호환성을 신경 쓰지 않게 되고, 폴리필을 사용해서 자바스크립트를 변환할 필요가 없어지는 때가 올거라 예상한다.

그러면 트랜스파일링 작업이 오로지 TS를 JS로 변환하는 작업 만을 위한 것이 되어 불필요한 작업성 저하라는 의견이 있다.

하지만 타겟 사용자가 구형 브라우저를 사용할 확률이 높거나, 이런 사용자가 다수 포함되어 있는 오래된 서비스나 대기업 제공 서비스들은 어쩔 수 없이 오랜 시간 동안 브라우저 호환성을 신경 쓸 수 밖에 없으며, 에버 그린 브라우저가 대세가 되기 위해서는 아직 시간이 좀 더 걸릴 것으로 보여 TS가 한동안 기본적으로 쓰이는 언어일 것이라고 예상한다.

## 사용 방법

---

### 설치

```jsx
npm install -g typescript
```

### 컴파일

- helloworld.ts 파일을 helloworld.js로 변환하는 방법

```jsx
tsc helloworld.ts // 한번만 컴파일
tsc helloworld.ts -w // 변경 사항을 추적해서 컴파일
tsc helloworld.ts --watch // 위와 같음
```

- 프로젝트 전체에 있는 ts파일 컴파일 하기

```tsx
tsc --init // 프로젝트를 타입스크립트로 관리되도록함. tsconfig.json 파일 생성
tsc // 프로젝트의 모든 ts파일 한번 컴파일
tsc -w // 프로젝트의 모든 ts파일 변경사항 추적해서 컴파일
tsc --watch // 위와 같음
```

### 구성 파일

- tsconfig.json에서 타입스크립트 컴파일과 관련된 설정을 해줄 수 있음

```tsx
tsc --init // tsconfig.json 생성됨
```

- 컴파일러 파일 관리 옵션

```tsx
// 특정 폴더, 파일만 컴파일에서 제외
"exclude": [
	"analytics.ts" // 이 ts 파일을 컴파일에서 제외
	"*.dev.ts" // 와일드카드를 사용해서 dev.ts파일 모두 제외
	// ❗exclude를 설정하지 않으면 node_modules는 기본적으로 컴파일 제외된다.
	"node_modules" // node_modules에 있는 ts파일 제외
]
// 특정 폴더, 파일만 컴파일
"include": [
	"..."
]
// 특정 파일만 컴파일
"files": [
	"..."
]
```

- 컴파일러 옵션

```tsx
// <https://www.typescriptlang.org/tsconfig/>
// 오픈 소스들 참고
// ex) <https://github.com/drizzle-team/drizzle-orm/blob/main/tsconfig.json>

"compilerOptions": {
	"target": "es5", // 해당 자바스크립트 버전으로 컴파일
	"module": "commonjs", // 모듈 방식
	"lib": [] // 기본값: target에 따라 다르지만 브라우저에서 사용가능한 API들을 사용할 수 있도록 하는 설정
	"allowJs": true, // JS파일을 TS가 컴파일 함
	"checkJs": true, // JS파일의 구문 검사, 잠재적 오류 보고
	"jsx": "preserve" // jsx 변환과 관련된 속성?
	"sourceMap": true // 디버깅에 도움을 줌
	"rootDir": "" // 컴파일 대상 파일이 있는 경로
	"outDir": "" // 컴파일된 js파일을 넣을 경로
	"removeComments": true // 컴파일 하면서 주석 제거
	"noEmit": true, // JS파일을 생성하지 않고 잠재적 오류만 보고
	"noEmitOnError": true // 오류 발견시 컴파일 하지 않기
	"strict": true, // any, null체크 등 엄격한 검사 수행
}
```

## 타입 추론**(Type Inference)**

---

- 타입을 명시적으로 할당하지 않아도 타입스크립트는 내장된 ‘타입 추론’이라는 기능을 활용해 타입을 추론해준다.
- 리터럴 타입을 변수에 할당해주면 된다.

![[Pasted image 20250212162753.png]]

자동으로 타입을 추론해줌

```tsx
const number = 5;

const run = (a: number, b: number): number => a + b;
const run = (a: number, b: 0): number => a + b;
const run = (a: 0, b: 0): number => a + b;
const run = (a: 0, b: 0): 0 => a + b;

// 정적 <-> 컴파일타임
// 동적 <-> 런타임

const response = await fetch("api");

const dataSchema = z.object({ age: z.number() });

const data = await response.json();
try {
  dataSchema.parse(data);
} catch {
  sentry.assert("서버 개발자가 이상합니다.");
  // 타입이 이상합니다.
}

data; // {age: number}

const myAge = data.age;
console.log(myAge);
```

- 만약 리터럴 타입을 바로 초기화하는 데 number타입을 할당하는 것은 중복되는 작업이므로 별로 좋은 방식은 아니다.
- 아래와 같이 타입을 할당해주어야 하는 경우는 바로 변수의 값을 초기화 해주지 않는 경우이다.

```jsx
const number: number = 5;
```

- 만약 아무 타입도 할당해주지 않고 초기화도 해주지 않으면 아까 살펴봤던 것처럼 any 타입이 할당되고, 아무 값이나 다 할당할 수 있다.

```jsx
let number;
number = 1;
number = "하이";
```

## 타입스크립트 기본 타입

---

### string, boolean, number, array, object

```jsx
let car: string = "bmw";
let isAdult: boolean = true;
let a: number = 1;

let a: number[] = [1, 2, 3]; // 기본 선언
let a2: Array<number> = [1, 2, 3]; // 제네릭 방식 활용

let week1: string[] = ["mon", "tue", "wed"]; // 기본 선언
let week2: Array<string> = ["mon", "tue", "wed"]; // 제네릭 방식 활용

// object 타입임을 명시
let user: object;

user = {
  name: "이은영",
  age: 25,
};

// 타입 추론을 할 수 있도록 리터럴 할당
let user = {
  name: "이은영",
  age: 25,
};

// 직접 타입 선언: 중복 작업이므로 좋지 않음
let person: {
  name: string,
  age: number,
} = {
  name: "이은영",
  age: 25,
};
```

### any

- 만약 아래처럼 아무런 타입도 명시해주지 않고 값을 초기화 하지 않으면 any 타입이 할당된다.
- any 타입은 모든 타입을 허용하기 때문에 어떤 값을 할당해도 타입스크립트가 에러를 발생시키지 않는다.
- 그러나 any타입을 남발하면 타입스크립트를 사용하는 이유가 없기 때문에 any 타입은 최대한 지양하도록 하자.

```jsx
let anytype;
anytype = 1;
anytype = "하이";
```

![자동으로 any 타입을 할당하고, 어떤 값을 할당해도 에러가 발생하지 않는다.](https://prod-files-secure.s3.us-west-2.amazonaws.com/e2b6748e-ba74-4d54-a235-94c36f9d61cb/b389d82e-0d71-4ded-a77a-c380a43fc3b7/Untitled.png)

자동으로 any 타입을 할당하고, 어떤 값을 할당해도 에러가 발생하지 않는다.

### Tuple

```jsx
// 튜플 (Tuple): 배열의 길이가 고정되고 각 요소의 타입이 지정되어 있는 배열 형식
let b: [string, number] = ["z", 1];
```

### void, never

```jsx
// void: 아무것도 반환하지 않는 함수
function sayHello(): void {
	console.log("hello")
}

// never: 항상 에러를 반환하거나 영원히 끝나지 않는 함수
function showError(): never{
	throw new Error();
}

function infLoop(): never{
	while(true){
		...
	}
}
```

### enum

```jsx
// enum: 상수의 집합 표현
enum OS {
	Window,
	Ios,
	Android
}

// 속성의 값을 주지 않으면 자동으로 0부터 1씩 증가하면서 속성 값이 할당됨
console.log(OS.Window, OS.Ios, OS.Android) // 1, 2, 3

enum OS {
	Window = 3,
	Ios,
	Android
}

// OS.Window 속성에 3을 주면 아래 속성들은 자동 오름차순
console.log(OS.Window, OS.Ios, OS.Android) // 3, 4, 5

enum OS {
	Window = 3,
	Ios = 10,
	Android
}

// 그 아래 속성도 마찬가지
console.log(OS.Window, OS.Ios, OS.Android) // 3, 10, 11

// 타입스크립트는 위 enum을 자바스크립트로 바꿀 때 아래처럼 바꿈
// 양방향 매핑되어 있음
var OS;
(function (OS){
	OS[OS["Window"] = 3] = "Window";
	OS[OS["Ios"] = 10] = "Ios";
	OS[OS["Android"] = 11] = "Android";
})(OS || (OS = {}));

// 따라서 양방향으로 접근 가능
console.log(OS[10]) // Ios
console.log(OS['Ios']) // 10

// 만약 숫자가 아닌 문자열을 할당하면 단방향 접근만 가능
enum OS {
	Window = 'win',
	Ios = 'ios',
	Android = 'and'
}

// 아래처럼 변환 (단방향 매핑)
var OS;
(function (OS){
	OS["Window"] = "win";
	OS["Ios"] = "ios";
	OS["Android"] = "and";
})(OS || (OS = {}));

// 만약 이 enum 타입을 변수 타입으로 선언해주면
let myOS: OS;

// myOS에는 enum에 있는 값만 할당할 수 있음. enum값만 할당할 수 있도록 제한됨.
myOS = OS.Window;
```

### null, undefined

```jsx
// null, undefined
let a: null = null;
let b: undefined = undefined;
```

## 타입 별칭 (Type Aliases)

---

- 타입스크립트의 type 키워드를 사용하면 특정 타입이나 인터페이스를 참조할 수 있는 **타입 변수**를 선언할 수 있다.

```tsx
type combinable = number | string; <-- 이렇게 숫자나 문자열이라고 표현하는 것 유니언 타입이라고 함
```

## 인터페이스 (interface)

---

- 인터페이스는 상호 간에 정의한 약속 혹은 규칙을 의미함
- 타입스크립트에서의 인터페이스는 보통 다음과 같은 범주에 대해 약속을 정의 가능
  - 객체의 스펙 (속성, 속성의 타입)
  - 함수의 파라미터
  - 함수의 스펙 (파라미터, 반환 타입 등)
  - 배열과 객체를 접근하는 방식
  - 클래스

```jsx
let user: object;

user = {
  name: "이은영",
  age: 30,
};

// user의 프로퍼티 타입을 명시하지 않으면 접근할 수 없다.
console.log(user.name); // Property 'name' does not exist on type 'object'.

// 1. 미리 프로퍼티를 정의해줄 때는 interface를 사용한다.
interface User {
  name: string;
  age: number;
}

let user: User = {
  name: "이은영",
  age: 30,
};

console.log(user.age); // 30
```

```tsx
// 2. 있어도 되고 없어도 되는 프로퍼티는 옵셔널로 표현한다 (?)
interface User {
  name: string;
  age: number;
  gender?: string; // <-- 옵셔널
}
// 없어도 되고
let user: User = {
  name: "이은영",
  age: 30,
};
// 있어도 됨
let user: User = {
  name: "이은영",
  age: 30,
  gender: "male",
};
```

```tsx
// 3. 읽기 전용 속성을 만들 때는 readonly를 프로퍼티 앞에 사용한다.
interface User {
  name: string;
  age: number;
  readonly birthYear: number; // <-- 읽기 전용
}

let user: User = {
  name: "이은영",
  age: 25,
  birthYear: 2000,
};

// readonly 속성은 변경할 수 없음
user.birthYear = 1990; // Cannot assign to 'birthYear' because it is a read-only property.
```

```tsx
// 4. 같은 타입을 여러개 두고 싶으면 아래와 같이 사용한다.
interface User {
  name: string;
  age: number;
  [grade: number]: string; // <-- number key: string value 프로퍼티를 여러개 추가 가능
}

let user: User = {
  name: "이은영",
  age: 25,
  1: "A",
  2: "B",
  3: "C",
};
```

```tsx
// 5. 함수도 interface로 정의할 수 있음
interface Add {
  (num1: number, num2: number): number;
}

const add: Add = function (x, y) {
  return x + y;
};
```

```tsx
// 6. 클래스도 정의할 수 있음
interface Car {
  color: string;
  wheels: number;
  start(): void;
}

class Bmw implements Car {
  // 클래스의 인터페이스로 Car 사용
  color;
  wheels = 4;
  constructor(c: string) {
    this.color = c;
  }
  start() {
    console.log("go...");
  }
}
```

```tsx
// 7. 인터페이스를 확장할 수 있음
interface Benz extends Car {
  door: number;
  stop(): void;
}

// 8. 동시에 여러개를 확장할 수도 있음
interface Car {
  color: string;
  wheels: number;
  start(): void;
}

interface Toy {
  name: string;
}

interface ToyCar extends Car, Toy {
  price: number;
}
```

## type vs interface

---

[https://yceffort.kr/2021/03/typescript-interface-vs-type](https://yceffort.kr/2021/03/typescript-interface-vs-type)

[https://velog.io/@wlwl99/TypeScript-type과-interface의-차이](https://velog.io/@wlwl99/TypeScript-type%EA%B3%BC-interface%EC%9D%98-%EC%B0%A8%EC%9D%B4)

### 1. 자료형

- `type`은 타입에 대한 별칭을 지어주기 위해 사용
- **모든 타입에 대해 사용 가능**

```tsx
type Point = {
  // 객체는 interface를 사용하는 것이 좋음
  x: number;
  y: number;
};

type Name = string; // primitive
type Age = number;
type Person = [string, number, boolean]; // tuple
type NumberString = string | number; // union
```

- `interface`는 type과 비슷하게 타입에 이름을 지정해주는 또 다른 방법
- **객체에만 사용 가능, 원시 자료형에 사용 불가능**

```tsx
interface Point {
  x: number;
  y: number;
}

interface name extends string { // ❌ 불가능
 ...
}
```

### 2. 확장성

- interface는 extends를 이용해 확장 가능

```tsx
interface Animal {
  name: string;
}

interface Bear extends Animal {
  honey: boolean;
}

const bear = getBear();
bear.name;
bear.honey;
```

- type은 인터섹션(`&`)를 통해 확장한 것과 동일하게 만들 수 있음 (상속이 아닌 기존 타입에 추가해서 새로운 타입을 정의

```tsx
type Animal = {
  name: string;
};

type Bear = Animal & {
  honey: Boolean;
};

const bear = getBear();
bear.name;
bear.honey;
```

### 3. 새 필드 추가

- interface는 **새 필드를 추가할 수 있음**. 동일한 이름의 인터페이스를 선언해서 추가할 타입을 작성하면 알아서 확장됨. (선언적 확장)

```tsx
interface Window {
  title: string;
}

interface Window {
  id: string;
}
/* 
interface Window{
  title: string;
  id: string;
}
*/
```

- type은 생성한 이후에는 **변경할 수 없음**

```tsx
type Window = {
  title: string;
};

type Window = {
  ts: TypeScriptAPI;
};

// Error: Duplicate identifier 'Window'.
```

### 4. computed value 사용 (계산된 값)

- interface에서는 **사용할 수 없음**

```tsx
type Subjects = "math" | "science" | "sociology";

interface Grades {
  [key in Subjects]: string; // ❗️Error: A mapped type may not declare properties or methods.
}
```

- type에서는 **사용 가능**

```tsx
type Subjects = "Math" | "Science" | "Sociology";

type Grades = {
  [key in Subjects]: string;
};
```

<aside> 💡 **결론** 내가 느낀 것은 `interface`는 객체를 위해서 만든 것이고 확장성이 좋으니까, **객체 타입을 정해줄 때**는 `interface`를 써주는 것이 좋을 것 같고, `type`과 비교해서 **타입을 ‘정의’, ‘약속’, 어떤 기능을 제공하는가 정의서를 제공**하는 느낌으로 와닿았다.

`type`은 타입 별칭이라는 말처럼 **하나의 변수 등이 여러가지 타입을 가질 때**, 길게 늘여 쓰지 않고 ‘축약’해서 타입을 사용할 수 있도록 하는 기능이라고 느꼈다. 따라서 객체 같은 자료형보다는 **원시값, 튜플, 유니언 타입 등을 선언할 때** 사용하는 것이 좋은 것 같다.

</aside>

## 제네릭 (Generic)

---

- 제네릭은 **사용하는 쪽에서 타입을 결정하고 싶을 때** 사용

```tsx
// 1. 함수 매개변수에 사용하기
function getSize<T>(arr: T[]): number {
  return arr.length;
}

const arr1 = [1, 2, 3];
getSize<number>(arr1); // 3

const arr2 = ["a", "b", "c"];
getSize<string>(arr2); // 3

// 사실 타입 추론에 의해서 타입을 할당해주지 않아도 됨
const arr3 = [false, true, true]; // <-- boolean[] 타입 추론
getSize(arr3); // 3 <-- 제네릭에 boolean이 자동으로 할당

// 아래와 같음
const arr3 = [false, true, true];
getSize<boolean>(arr3); // 3
```

```tsx
// 2. 인터페이스에 사용하기
interface Mobile<T> {
  name: string;
  price: number;
  option: T;
}

const m1: Mobile<object> = {
  name: "s21",
  price: 10000,
  option: {
    color: "red",
    coupon: false,
  },
};

const m2: Mobile<string> = {
  name: "s20",
  price: 900,
  option: "good",
};
```

```tsx
// 3. 객체에 프로퍼티가 있는 지 확인하는 함수 제네릭으로 만들기
// feat. **제네릭에 제약조건 걸기**
interface User {
  name: string;
  age: number;
}

interface Car {
  name: string;
  color: string;
}

interface Book {
  price: number;
}

const user: User = { name: "a", age: 10 };
const car: Car = { name: "bmw", color: "red" };
const book: Book = { price: 3000 };

// 객체의 name 프로퍼티에 접근해서 리턴하는 함수
// 제네릭을 아래와 같이 사용하면 -> data는 어떤 타입을 가지긴 하는데 { name: string }을
// 확장한 타입을 가질 것이다 미리 예고하는 것
function showName<T extends { name: string }>(data: T): string {
  return data.name;
}

showName(user); // a
showName(car); // bmw
showName(book); // Argument of type "Book" is not assignable to parameter of type {name: string}
```

## 유틸리티 타입 (Utility type)

---

[https://www.youtube.com/playlist?list=PLZKTXPmaJk8KhKQ_BILr1JKCJbR0EGlx0](https://www.youtube.com/playlist?list=PLZKTXPmaJk8KhKQ_BILr1JKCJbR0EGlx0) 참고

- 유틸리티 타입은 이미 정의해 놓은 타입을 변환할 때 사용하기 좋은 문법
- 유틸리티 타입을 꼭 쓰지 않아도 기존 인터페이스, 제네릭 등의 기본 문법으로도 타입을 변환할 수 있지만 훨씬 더 간결한 문법으로 타입을 정의할 수 있음

### 1. keyof: 키 값을 타입으로 변환

```tsx
interface User {
  id: number;
  name: string;
  age: number;
  gender: "m" | "f";
}

type Userkey = keyof User; // 'id' | 'name' | 'age' | 'gender'
const uk: UserKey = "";
```

### 2. Partial<T>: 옵셔널로 변환

```tsx
// Partial<T> : 객체의 모든 속성을 옵셔널로 변환

interface User {
  id: number;
  name: string;
  age: number;
  gender: "m" | "f";
}

let admin: User = {
  // ❌ 모든 옵션이 선언되지 않아 에러
  id: 1,
  name: "Bob",
};

let admin: Partial<User> = {
  // ✅ User 타입의 속성을 옵셔널로 모두 변환
  id: 1,
  name: "Bob",
};

// Partial<User>는 아래와 같이 타입을 선언한 것과 같음
interface User {
  id?: number;
  name?: string;
  age?: number;
  gender?: "m" | "f";
}

// 따라서 없는 프로퍼티를 사용하려고 하면 에러 발생
let admin: Partial<User> = {
  id: 1,
  name: "Bob",
  job: "", // ❌ User interface에 없는 속성이기 때문에 에러
};
```

### 3. Required<T>: 필수 속성으로 변환

```tsx
interface User {
  id: number;
  name: string;
  age?: number;
}

// ✅ User 인터페이스의 필수 속성을 모두 선언함
let admin: User = {
  id: 1,
  name: "bob",
};

// ❌ User 인터페이스의 모든 속성이 필수가 되어 age속성도 있어야 함
let admin: Required<User> = {
  id: 1,
  name: "bob",
};
```

### 4. Readonly<T>: 읽기 전용으로 변환

```tsx
interface User {
  id: number;
  name: string;
  age?: number;
}

let admin: Readonly<User> = {
  id: 1,
  name: "bob",
};

admin.id = 4; // ❌ readonly 속성으로 변경되어 변경 불가능
```

### 5. Record<K, T>: 반복되는 키, 값의 속성을 한번에 적용

```tsx
// 0. 이 코드를
interface Score {
	"1": "A" | "B" | "C" | "D"';
	"2": "A" | "B" | "C" | "D"';
	"3": "A" | "B" | "C" | "D"';
	"4": "A" | "B" | "C" | "D"';
}

const score: Score = {
	1: "A",
	2: "C",
	3: "B",
	4: "D",
}

// 1. 이렇게 변경 가능 ✨
const score: Record<"1" | "2" | "3" | "4", "A" | "B" | "C" | "D"> = {
	1: "A",
	2: "C",
	3: "B",
	4: "D",
}

// 2. 그리고 type으로 분리하면 더 깔끔해짐 ✨✨✨
type Grade = "1" | "2" | "3" | "4";
type Score = "A" | "B" | "C" | "D";

const score: Record<Grade, Score> = {
	1: "A",
	2: "C",
	3: "B",
	4: "D",
}
```

```tsx
interface User {
	id: number;
	name: string;
	age: number;
}

// 이런 함수가 있다고 했을 때
function isValid(user: User){
	const result = {
		id: user.id > 0,
		name: user.name !== '',
		age: user.age > 0
	}
	return result;
}

// result 객체는 아래처럼 타입을 명시해줄 수 있음
// User의 key들을 key로 사용하고, 값은 boolean값이다~
function isValid(user: User){
	const result: **Record<keyof User, boolean>** = {
		id: user.id > 0,
		name: user.name !== '',
		age: user.age > 0
	}
	return result;
}
```

### 6. Pick<T, K>: 타입에서 특정 키만 갖고 오기

```tsx
interface User {
  id: number;
  name: string;
  age: number;
  gender: "M" | "W";
}

// User 인터페이스에서 id, name 키만 갖고와서 쓰겠다는 의미
const admin: Pick<User, "id" | "name"> = {
  id: 0,
  name: "Bob",
};
```

### 7. Omit<T, K>: 타입에서 특정 키만 생략하기

```tsx
interface User {
  id: number;
  name: string;
  age: number;
  gender: "M" | "W";
}

// User 인터페이스에서 age와 gender만 제외하고 사용하겠다는 의미
const admin: Omit<User, "age" | "gender"> = {
  id: 0,
  name: "Bob",
};
```

### 8. Exclude<T1, T2>: T1에서 T2타입을 제거하기

```tsx
type T1 = string | number;
// T1에서 number 타입을 제거함
type T2 = Exclude<T1, number>; // T2는 string 타입이 됨
```

### 9. NonNullable<Type>: 타입에서 null, undefined를 제외하기

```tsx
type T1 = string | undefined | void;
type T2 = NonNullable<T1>; // string | void 만 남음
```

## 타입 내로잉 (Type Narrowing)

---

[https://medium.com/nodejs-server/type-narrowing-ts-f62fc28f413f](https://medium.com/nodejs-server/type-narrowing-ts-f62fc28f413f) 참고

- Narrowing은 범위를 좁히는 것을 의미한다.
- 타입스크립트에서 변수는 덜 정확한 타입에서 더 정확한 타입으로 변할 수 있다.
- 이 과정을 _**type narrowing**_(타입 좁힘)라고 한다.
- 특히 Union 타입에서 더욱 구체적인 타입으로 논리적으로 유추할 수 있다.
- Narrowing의 ~~종류~~ 예시는 다음과 같다.
  - Assignment Narrowing
  - Typeof Narrowing
  - Truthiness Narrowing
  - Equality Narrowing
  - In operator Narrowing
  - Instanceof Narrowing
  - Discriminated union Narrowing (차별된 유니언 내로잉)
  - Exhaustiveness checking

### Assignment Narrowing (할당 축소)

- 값을 할당하는 순간 Narrowing이 적용되어 타입 유추가 가능해지는 것을 말한다.

```tsx
let numberOrString: number | string = "abc"; // 값이 할당되어 string 타입이 됨
numberOrString.toUpperCase(); // string 타입에만 존재하는 메소드 사용 가능
```

### Typeof Narrowing (할당 축소)

- 자바스크립트의 typeof 키워드를 통해 직접적으로 Narrowing하는 방식이다.

```tsx
let numberOrString: number | string;
numberOrString = ... // 두 타입 중 하나 할당

if(typeof numberOrString === 'string'){
	numberOrString.toUpperCase();
} else {
	numberOrString.toFixed(2);
}
```

### Truthiness Narrowing (진실 축소)

- `if()` 조건문에서 `true`로 걸린다면 축소되는 방식이다.

```tsx
let numberOrString: null | string[];
numberOrString = ... // 두 타입 중 하나 할당

if (nullOrString) {
  nullOrString; // string[] 취급을 받음 (null 은 if() 를 통과하지 못함)
} else {
  nullOrString; // null 취급을 받음 (if() 를 통과하지 못했으면 null 임)
}
```

### **Equality Narrowing** (평등 축소)

- `===` (Equality) 키워드를 통해 축소되는 방식이다.

```tsx
let numOrStr: number | string;
let numOrBool: number | boolean;

if (numOrStr === numOrBool) {
  // 둘이 타입이 같으므로 여기서는 둘다 number 취급
  numOrStr;
  numOrBool;
} else {
  // 둘이 타입이 같지 않으므로 number이면 boolean, string이면 number나 boolean
  numOrStr;
  numOrBool;
}
```

### **In operator Narrowing** (in 연산자 축소)

- 자바스크립트에서 해당 프로퍼티가 존재하는 지 확인할 수 있는 `in`키워드를 통해 축소하는 방식이다.

```tsx
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
  if ("swim" in animal) {
    return animal.swim();
  }
  return animal.fly();
}

type Fish = { swim: () => void };
type Bird = { fly: () => void };
type Human = { swim?: () => void; fly?: () => void };

function move(animal: Fish | Bird | Human) {
  if ("swim" in animal) {
    animal; // (parameter) animal: Fish | Human
  } else {
    animal; // (parameter) animal: Bird | Human
  }
}
```

### **instanceof Narrowing** (in 연산자 축소)

- 어떤 클래스를 인스턴스화 시켰는지 확인할 수 있는 `instanceof`키워드를 통해 축소하는 방식이다.

```tsx
let dateOrString: Date | string;

if (dateOrString instanceof Date) {
  dateOrString; // Date 타입 취급
} else {
  dateOrString; // string 타입 취급
}
```

### **Discriminated Union Narrowing** (구분된 유니언 축소)

- **유니언 타입**의 **구체적인 하위 타입**으로 값을 **추론**하는 기술을 말한다.
- 객체의 속성을 옵셔널 `?` 을 통해 모호하게 타입을 나누는 것을 제거하고 더 명확히 타입을 나누는 것이 좋다.
- 타입 설계 시 옵셔널 사용은 피하는 것이 best practice이다.

```tsx
interface SmartPhone {
  type: "galaxy" | "iphone";
  appleLogo?: true;
  samsungLogo?: true;
}

let phone: SmartPhone = random()
  ? {
      type: "galaxy",
      samsungLogo: true,
    }
  : {
      type: "iphone",
      appleLogo: true,
    };

if (phone.type === "galaxy") {
  phone.samsungLogo; // 추론 불가 (true | undefined)
} else {
  phone.appleLogo; // 추론 불가 (true | undefined)
}

interface Galaxy {
  type: "galaxy";
  samsungLogo: true;
}

interface Iphone {
  type: "iphone";
  appleLogo: true;
}

type GalaxyOrIphone = Galaxy | Iphone;

// 이전에 phone 변수에 할당했던 것과 동일하게 할당
let galaxyOrIphone: GalaxyOrIphone = random()
  ? {
      type: "galaxy",
      samsungLogo: true,
    }
  : {
      type: "iphone",
      appleLogo: true,
    };

if (galaxyOrIphone.type === "galaxy") {
  galaxyOrIphone; // Galaxy 로 정상적으로 추론됨
} else {
  galaxyOrIphone; // Iphone 으로 정상적으로 추론됨
}
```

### **Exhaustiveness Checking** (완전성 검사)

- switch … case 문에서 유용하게 사용 가능하다.
- case에 모든 type을 검증했는지 아래의 never 타입 체크에서 확실히 검증된다.

```tsx
switch (galaxyOrIphone.type) {
  case "galaxy":
    galaxyOrIphone; // Galaxy 타입
    break;
  case "iphone":
    galaxyOrIphone; // Iphone 타입
    break;
  default:
    galaxyOrIphone; // never 타입
    // 나중에 Galaxy, Iphone 이후에 Xiaomi 타입도 올 수 있는데
    // 이 때 확실히 모든 타입을 case 에서 처리했는지 검증 가능
    // 타입스크립트를 스마트하게 활용할 수 있는 방법 중 하나임
    const _check: never = galaxyOrIphone;
    break;
}
```

# Reference

[https://www.youtube.com/watch?v=9K4EL1jeSmk](https://www.youtube.com/watch?v=9K4EL1jeSmk)
