# 타입스크립트

## 📌 Notion 문서 링크
[📖 Notion 문서 바로가기](https://bokjunwoo.notion.site/TypeScript-19577d2ca3638066bc5fe4823fa1588b)

## 🔎 미리보기
## 타입스크립트가 등장한 이유

### **자바스크립트의 문제점**

JavaScript는 **동적 타입 언어**로, 변수의 타입이 실행 중 결정됩니다.

이로 인해 다음과 같은 문제가 발생합니다.

1. **런타임 오류 발생 가능**
    
    ```jsx
    function add(a, b) {
      return a + b;
    }
    console.log(add(5, "10")); // 결과: "510" (예상과 다름)
    ```
    
    - 숫자를 더할 줄 알았는데, 문자열 연결이 발생
2. **코드 자동 완성이 어려움**
    - 객체에 어떤 속성이 있는지 IDE가 알기 어려움.
    - 실수로 잘못된 속성을 입력해도 검출되지 않음.
3. **규모가 커질수록 유지보수 어려움**
    - 대규모 프로젝트에서는 변수와 함수의 타입을 명확히 알기 어려워 버그가 증가.

### **타입스크립트의 등장**

타입스크립트는 **정적 타입 검사(Static Type Checking)** 를 도입하여 위 문제를 해결합니다.

- **컴파일 시 타입 오류 감지**
- **코드 자동 완성 및 IDE 지원 강화**
- **객체, 함수, 변수의 타입을 명확하게 정의하여 유지보수성 향상**
- **대규모 프로젝트에서 안정적인 개발 가능**

## 타입스크립트 컴파일 과정 (TypeScript Compilation Process)

타입스크립트(TypeScript)는 정적 타입 검사(Static Type Checking)를 제공하지만, 브라우저와 Node.js는 타입스크립트를 직접 실행할 수 없습니다.
따라서 **타입스크립트 코드를 자바스크립트 코드로 변환하는 컴파일 과정이 필요**합니다.
이 과정에서 **타입 체크(Type Checking)**, **트랜스파일링(Transpiling, 문법 변환)**, 최적화(Optimization)가 이루어집니다.

## **컴파일 과정 단계**

### **1. 코드 작성 (`.ts` 파일)**

📌 **예제**

```tsx
const message: string = "Hello, TypeScript!";
```

### **2. 타입 검사 (Type Checking)**

- 타입스크립트는 **코드를 실행하기 전에 타입 오류를 검사**합니다.
- `tsc`(TypeScript Compiler)가 **변수, 함수, 객체 등의 타입이 올바르게 사용되었는지 확인**합니다.
- 타입 오류가 발생하면 컴파일이 실패합니다.

📌 **타입 오류 예제**

```tsx
const num: number = "Hello"; // ❌ 오류: 'string'을 'number' 타입에 할당할 수 없음
```

📌 **타입 검사 결과**

```tsx
error TS2322: Type 'string' is not assignable to type 'number'.
```

**타입 검사를 통해 런타임 오류를 미리 방지**할 수 있습니다.

### **3. 트랜스파일링 (Transpiling, 문법 변환)**

- 타입스크립트는 ES6+ 문법을 지원하지만, 구형 브라우저에서는 ES6 문법을 지원하지 않을 수 있습니다.
- `tsc`는 **최신 타입스크립트 코드(ES6+)를 하위 버전의 JavaScript(ES5, ES3 등)로 변환**합니다.
- **트랜스파일링(Transpiling)** 과정에서 타입 정보를 제거하고, **순수한 JavaScript 코드로 변환**됩니다.

📌 **컴파일 결과 (`index.js` - ES5 변환)**

```jsx
"use strict";
var message = "Hello, TypeScript!";
```

**자바스크립트에서 타입 정보(`: string` 등)는 사라지고, 실행 가능한 코드만 남음.**

### **4. 최적화 (Optimization)**

컴파일러는 **코드를 최적화하여 실행 성능을 향상**시킵니다.

- **타입 제거**: 런타임에서 필요하지 않은 타입 정보 삭제.
- **불필요한 코드 제거**: 사용되지 않는 변수/함수 삭제.
- **모듈 번들링 지원**: 여러 파일을 하나로 합쳐 성능 최적화 가능.

📌 **예제**

```tsx
const unusedVar: number = 100; // 사용되지 않음
console.log("Optimized Code");
```

📌 **컴파일 결과 (`input.js` - 최적화 후)**

```jsx
console.log("Optimized Code");
```

**사용되지 않는 코드(`unusedVar`)가 제거됨.**

## **`tsconfig.json`을 이용한 컴파일 설정**

### **1. `tsconfig.json` 생성**

```tsx
tsc --init
```

📌 **생성된 `tsconfig.json` 예제**

```json
{
  "compilerOptions": {
    "target": "ES5", // 컴파일 후 변환할 JavaScript 버전 (ES3, ES5, ES6, ESNext)
    "module": "CommonJS", // 모듈 시스템 설정 (CommonJS, ESNext, AMD 등)
    "strict": true, // 엄격한 타입 검사 활성화
    "outDir": "./dist", // 컴파일된 JS 파일 저장 경로
    "rootDir": "./src", // TS 소스 파일 경로
    "noImplicitAny": true // any 타입 사용 금지
  }
}
```

**설정된 옵션에 따라 자동으로 타입스크립트가 변환됨.**

# 주요 개념

## **타입 추론 (Type Inference)**

### **타입 추론이란?**

- 타입스크립트는 변수 선언 시 **명시적인 타입 지정 없이도 타입을 자동으로 유추**합니다.
- **타입을 생략해도 타입스크립트가 알아서 타입을 결정**해 줍니다.

### **1. 기본 타입 추론**

```tsx
let num = 10; // number로 추론
let text = "Hello"; // string으로 추론
let isActive = true; // boolean으로 추론
```

타입을 명시하지 않아도 **자동으로 타입이 결정됨**.

### **2. 함수 반환 타입 추론**

- 함수의 반환 타입도 자동으로 추론됩니다.

```tsx
function add(a: number, b: number) {
  return a + b; // 반환 타입: number
}
```

`add()` 함수는 **숫자를 반환하므로 number 타입으로 추론**됨.

### **3. 객체 타입 추론**

```tsx
const user = {
  name: "BokJunWoo",
  age: 27,
};
```

 `user` 객체의 타입은 **자동으로 `{ name: string; age: number }`로 추론됨**.

### **4. 문맥적 타입 추론**

- **이벤트 핸들러에서 타입을 추론**할 수도 있음.

```tsx
window.addEventListener("click", (event) => {
  console.log(event.clientX); // `event`는 MouseEvent로 추론됨
});
```

**문맥(Context)에 따라 타입이 결정됨**.

## **타입 호환 (Type Compatibility)**

### **타입 호환이란?**

- 타입스크립트에서 **한 타입을 다른 타입에 할당할 수 있는지 여부를 결정하는 규칙**.
- **구조적 타이핑(Structural Typing)** 을 사용하여 **객체의 속성과 타입이 일치하면 호환 가능**.

### **1. 기본 타입 호환**

```tsx
let num1: number = 10;
let num2: number | string = num1; // ✅ number → number | string (가능)
```

**더 넓은 범위의 타입(`number | string`)에 number를 할당할 수 있음**.

### **2. 객체 타입 호환**

```tsx
type Person = {
  name: string;
  age: number;
};

type Friend= {
  name: string;
  age: number;
  position: string;
};

let person: Person = { name: "BokJunWoo", age: 27 };
let friend: Friend = { name: "Bob", age: 30, position: "Developer" };

person = friend; // ✅ 가능 (Employee는 Person의 모든 속성을 포함)
friend = person; // ❌ 불가능 (Employee에는 position이 필요)
```

**속성이 더 많은 객체는 더 적은 속성을 가진 타입에 할당 가능**.

**하지만 속성이 부족한 객체는 더 많은 속성을 가진 타입에 할당 불가능**.

### **3. 함수 타입 호환**

- 매개변수 개수가 적은 함수는 개수가 많은 함수에 할당 가능.

```tsx
let func1 = (a: number) => a * 2;
let func2 = (a: number, b: number) => a + b;

func2 = func1; // ❌ 불가능 (매개변수 개수가 다름)
func1 = func2; // ✅ 가능 (func1은 필요한 매개변수만 받으면 됨)
```

## **타입 별칭 (Type Alias)**

### **타입 별칭이란?**

- `type` 키워드를 사용하여 **재사용 가능한 타입을 정의**할 수 있음.

### **1. 기본 타입 별칭**

```tsx
type User = {
  name: string;
  age: number;
};

const user1: User = { name: "BokJunWoo", age: 27 };
```

**여러 곳에서 동일한 타입을 사용할 때 코드 중복을 줄일 수 있음**.

---

### **2. 유니온 타입과 결합**

```tsx
type Status = "success" | "error" | "loading";

let apiStatus: Status = "success"; // ✅ 가능
apiStatus = "error"; // ✅ 가능
apiStatus = "failed"; // ❌ 오류 (정의되지 않은 값)
```

**유니온 타입을 사용하면 특정 값만 허용 가능**.

### **3. 함수 타입 별칭**

```tsx
type AddFunction = (a: number, b: number) => number;

const add: AddFunction = (x, y) => x + y;
```

**함수 타입도 별칭으로 정의 가능**.

## **타입 단언 (Type Assertion)**

### **타입 단언이란?**

- 개발자가 **"이 값은 특정 타입이다"** 라고 단언하는 기능.
- `as` 또는 `<타입>`을 사용하여 타입을 강제할 수 있음.

### **1. `as`를 사용한 타입 단언**

```tsx
let value: any = "Hello";
let length: number = (value as string).length; // ✅ string으로 단언
```

## **타입 가드 (Type Guard)**

### **타입 가드란?**

- **대신에서 가치관의 유형을 검사하여 참석하는 방법**
- `typeof`, `instanceof`, **맞춤형 타입 가드** 를 활용합니다.

### **1. `typeof`를 활용한 타입 가드**

```tsx
function processValue(value: number | string) {
  if (typeof value === "string") {
    console.log(value.toUpperCase()); // ✅ 안전한 문자열 처리
  } else {
    console.log(value.toFixed(2)); // ✅ 안전한 숫자 처리
  }
```

### **2. `instanceof`를 활용한 타입 가드**

```tsx
class Dog {
  bark() {
    console.log("Woof!");
  }
}

class Cat {
  meow() {
    console.log("Meow!");
  }
}

function makeSound(animal: Dog | Cat) {
  if (animal instanceof Dog) {
    animal.bark(); // ✅ 안전하게 Dog의 메서드 사용 가능
  } else {
    animal.meow(); // ✅ 안전하게 Cat의 메서드 사용 가능
  }
}
```

# Type과 Interface의 차이점

## **`type`과 `interface`의 기본적인 차이점**

| 비교 항목 | `type` | `interface` |
| --- | --- | --- |
| **확장 가능 여부** | `extends` 불가능 (재정의해야 함) | `extends`로 확장 가능 |
| **유니온/교차 타입 지원** | ✅ 가능 (`type A = B | C`) |
| **객체, 함수, 원시 타입 정의** | ✅ 가능 | ❌ 객체 형태만 가능 |
| **성능** | 컴파일 성능이 약간 느림 | 컴파일 성능이 최적화됨 |
| **자동 병합 (Declaration Merging)** | ❌ 불가능 | ✅ 가능 (`interface`는 동일한 이름이면 자동 병합됨) |
- `interface`는 **객체의 구조를 정의**하는 데 최적화됨.
- `type`은 **유니온 타입, 함수 타입, 원시 타입 등을 정의할 때 유용**함.

## **기본적인 사용법**

### **`type` 사용 예제**

```tsx
type User = {
  id: number;
  name: string;
};

const user: User = { id: 1, name: "BokJunWoo" };
```

**객체의 타입을 정의할 때 사용 가능.**

### **`interface` 사용 예제**

```tsx
interface User {
  id: number;
  name: string;
}

const user: User = { id: 1, name: "BokJunWoo" };
```

 **`type`과 `interface` 모두 동일한 방식으로 객체 타입을 정의할 수 있음.**

## **`interface`의 확장 (`extends`)**

### **`interface`는 상속(확장)이 가능**

```tsx
interface Person {
  name: string;
  age: number;
}

interface Employee extends Person {
  position: string;
}

const emp: Employee = { name: "Alice", age: 30, position: "Developer" };
```

**`extends`를 사용하면 기존 인터페이스를 쉽게 확장 가능**.

**대규모 프로젝트에서 객체를 확장해야 할 경우 `interface`가 유리**.

### **`type`의 확장 (유니온 & 교차 타입)**

- `type`은 `extends`를 사용할 수 없지만, **교차 타입(&)을 사용하면 확장 가능**.

```tsx
type Person = {
  name: string;
  age: number;
};

type Employee = Person & {
  position: string;
};

const emp: Employee = { name: "Alice", age: 30, position: "Developer" };
```

**교차 타입(`&`)을 사용하면 `extends`와 비슷한 효과를 낼 수 있음.**

## **유니온 타입 지원 (`type`만 가능)**

- `type`은 **유니온(`|`)과 인터섹션(`&`)을 지원**하지만, `interface`는 지원하지 않음.

```tsx
type Status = "success" | "error" | "loading"; // ✅ 가능
```

```tsx
interface Status {
  status: "success" | "error" | "loading"; // ❌ 유니온 타입 사용 불가능
}
```

**유니온 타입을 사용할 때는 `type`을 쓰는 것이 좋음**.

## **함수 타입 정의**

### **함수 타입을 정의할 때는 `type`이 더 편리**

```tsx
type Add = (a: number, b: number) => number;

const add: Add = (x, y) => x + y;
```

```tsx
interface Add {
  (a: number, b: number): number;
}

const add: Add = (x, y) => x + y;
```

**둘 다 가능하지만, `type`을 쓰면 직관적으로 함수 타입을 정의할 수 있음.**

## **자동 병합 (Declaration Merging)**

- **`interface`는 동일한 이름이 여러 번 선언되면 자동으로 병합**됨.
- **`type`은 자동 병합이 불가능**.

### **`interface` 자동 병합**

```tsx
interface User {
  name: string;
}

interface User {
  age: number;
}

const user: User = { name: "Alice", age: 30 }; // ✅ 정상 동작
```

**`User` 인터페이스가 자동으로 병합됨**.

### **`type`은 자동 병합 불가능**

```
type User = { name: string };
type User = { age: number }; // ❌ 오류 발생 (동일한 타입 선언 불가능)
```

**같은 이름의 `type`을 중복 선언하면 오류 발생**.

# 제네릭 (Generics)

## **제네릭이란?**

제네릭(Generics)은 **타입을 변수처럼 다룰 수 있도록 해주는 기능**으로,

**재사용성이 높고 타입 안정성이 보장된 코드**를 작성할 수 있게 해줍니다.

### **제네릭이 필요한 이유**

일반적으로 **함수나 클래스는 특정 타입에 종속될 때가 많음**.

예를 들어, **숫자 배열을 처리하는 함수**를 만든다면 다음과 같이 작성할 수 있습니다.

```tsx
function getFirst(arr: number[]): number {
  return arr[0];
}
```

하지만 **문자열 배열을 처리하려면 또 다른 함수를 만들어야 함**.

```tsx
function getFirstString(arr: string[]): string {
  return arr[0];
}
```

**같은 로직인데도 타입이 달라서 중복 코드가 발생함.**

**이 문제를 해결하기 위해 제네릭을 사용하면 모든 타입을 지원하는 재사용 가능한 함수가 가능!**

## **기본적인 제네릭 문법**

### **제네릭 함수 사용법**

제네릭을 사용하면 **타입을 변수처럼 받아서 사용할 수 있음**.

```tsx
function getFirst<T>(arr: T[]): T {
  return arr[0];
}

console.log(getFirst<number>([1, 2, 3])); // 1
console.log(getFirst<string>(["a", "b", "c"])); // "a"
```

`T`는 타입 매개변수(타입 변수)로, 호출할 때 타입을 지정할 수 있음.

즉, `T`는 **넘겨받는 타입에 따라 자동으로 결정됨**.

### **제네릭 인터페이스**

```tsx
interface Box<T> {
  value: T;
}

const numberBox: Box<number> = { value: 42 };
const stringBox: Box<string> = { value: "Hello" };
```

**인터페이스에서도 제네릭을 사용할 수 있으며, 유연한 타입 지정이 가능!**

## **제네릭 타입 제한 (Constraints)**

제네릭을 사용할 때, **아무 타입이나 다 받을 수 있지만 특정 조건을 부여할 수도 있음**.

### **1. 특정 타입만 허용하기 (`extends` 사용)**

예를 들어, `length` 속성이 있는 타입만 받도록 제한할 수 있습니다.

```tsx
function logLength<T extends { length: number }>(item: T): void {
  console.log(item.length);
}

logLength("Hello"); // 5 (문자열은 length 속성이 있음)
logLength([1, 2, 3]); // 3 (배열도 length 속성이 있음)
// logLength(10); // ❌ 오류: 숫자는 length 속성이 없음
```

**`T extends { length: number }`를 사용하여 `length` 속성이 있는 타입만 받을 수 있도록 제한함.**

### **2. 제네릭에서 인터페이스 활용**

```tsx
interface User {
  name: string;
  age: number;
}

function getUserInfo<T extends User>(user: T): string {
  return `이름: ${user.name}, 나이: ${user.age}`;
}

const user = { name: "Alice", age: 30, role: "admin" }; // role 속성이 추가되어도 문제없음
console.log(getUserInfo(user)); // "이름: Alice, 나이: 30"
```

**인터페이스를 확장하여 특정 속성을 가진 객체만 받도록 제한할 수도 있음**.

## **여러 개의 제네릭 타입 사용**

제네릭을 사용할 때 **한 개 이상의 타입 변수를 사용할 수도 있음**.

```tsx
function pair<K, V>(key: K, value: V): [K, V] {
  return [key, value];
}

console.log(pair("id", 123)); // ["id", 123]
console.log(pair(true, { name: "Alice" })); // [true, { name: "Alice" }]
```

**제네릭을 여러 개 사용하면 서로 다른 타입을 받을 수 있음**.

## **제네릭과 유틸리티 타입**

타입스크립트의 **기본 제공 유틸리티 타입**도 제네릭을 활용하여 구현됩니다.

### **1. `Partial<T>` (모든 속성을 선택적(Optional)으로)**

```tsx
interface User {
  name: string;
  age: number;
}

const user: Partial<User> = {}; // ✅ 모든 속성이 선택적이 됨
user.name = "Alice";
```

**제네릭을 활용하여 특정 타입의 속성을 변경할 수 있음.**

### **2. `Readonly<T>` (모든 속성을 읽기 전용으로)**

```tsx
interface User {
  name: string;
  age: number;
}

const user: Readonly<User> = { name: "Alice", age: 30 };
// user.name = "Bob"; // ❌ 오류: Readonly이므로 변경 불가능
```

**객체의 속성을 변경할 수 없도록 만들 수 있음.**

### **3. `Record<K, V>` (키와 값을 특정 타입으로 제한)**

```tsx
type UserRole = "admin" | "user" | "guest";

const roles: Record<UserRole, string> = {
  admin: "관리자",
  user: "일반 사용자",
  guest: "방문자",
};
```

**특정 키와 값을 매핑할 때 유용함.**

## **제네릭은 언제 사용할까?**

- **여러 타입을 지원하면서 코드 중복을 줄이고 싶을 때**
- **타입 안정성을 유지하면서 재사용 가능한 코드를 만들고 싶을 때**
- **함수, 클래스, 인터페이스, 타입 별칭을 유연하게 사용하고 싶을 때**

# **`any` vs `unknown` 차이**

### **`any`**

- **모든 타입을 허용**하지만, **타입 안전성이 없음**.

```tsx
let value: any = 10;
value = "Hello"; // ❌ 타입 오류 감지 불가
```

### **`unknown`**

- **모든 타입을 허용하지만, 타입 검사를 요구함**.

```tsx
let value: unknown = "Hello";

if (typeof value === "string") {
  console.log(value.toUpperCase()); // ✅ 타입 검사 후 안전하게 사용 가능
}
```

**`unknown`은 타입 안전성이 높은 `any`의 대체재**.