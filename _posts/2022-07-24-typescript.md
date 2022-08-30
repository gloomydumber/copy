---
layout: post
title: "TypeScript"
date: 2022-07-24 09:00:00
categories: WEB-BACKEND
permalink: /archivers/typescript
nocomments: false
use_math: true
---

# TypeScript

| ![web3means](/assets/posts/2022-07-24-typescript/tslogo.png) |
| :----------------------------------------------------------: |
|                      <b>TypeScript</b>                       |

Studying TypeScript and Memo

## commands

### initialize

```bash
tsc init
```

`tsconfig.json` 생성

### watch mode

```bash
tsc ${filename} -w
```

원본 _.ts_ 파일이 수정될 때 마다, _.js_ 파일로 컴파일

## tsconfig.json

`tsconfig.json`의 *property*에 관해 정리

### outDir

`compilerOptions`, 컴파일 완료 된 `.js` 파일을 출력할 경로

### rootDir

`compilerOptions`, 컴파일을 실행할 위치, 지정한 위치 상위에 `.ts` 파일이 있으면 에러 반환

### incremental

`compilerOptions`, 이전 컴파일과 비교해서 수정된 사항에 안해서만 컴파일 실행

### target

`compilerOptions`, 어떤 버전으로 컴파일 할 것인지 설정. 하위호환을 위해 상황에 관계 없이 낮은 버전을 사용하는 것은 지양. `ES5` 혹은 `ES6`를 많이 씀

### module

`compilerOptions`, `Node.js` 프로젝트의 경우 `CommonJS` (import가 아닌 require 이용), 브라우저 환경일 경우 `ES` 표준에 맞게 설정

### lib

`compilerOptions`, 세부적으로 어떤 `library`를 이용할 것인지 설정. 보통은 따로 설정하지 않고 `target` 설정시 사용되는 기본 `lib`를 사용

### allowJs

`compilerOptions`, 프로젝트 안에서 `.js`를 같이 사용할 것인지 여부 설정

### checkJs

`compilerOptions`, `.js` 파일에서 에러가 있다면 경고 표시

### jsx

`compilerOptions`, `react`에서 사용되는 `.jsx` 파일에 관한 설정

### declaration

`compilerOptions`, `type` 정의 관련. 작성한 코드가 라이브러리로서 사용되는 경우에 주로 사용. 일반 프로젝트에서는 사용하지 않음

### sourceMap

`compilerOptions`, 디버깅을 위해서 사용

### outFile

`compilerOptions`, 작성한 여러 `.ts`를 하나의 `.js`로 컴파일할 때 사용

### composite

`compilerOptions`, `incremental`과 같이 사용. 빌드 정보를 저장하여 이후에 있을 빌드를 빠르게 할 수 있도록 함

### tsBuildInfoFIle

`compilerOptions`, `incremental`의 설정이 `true`일 경우 그에 관련된 정보를 저장하는 파일을 출력하도록 설정

### removeComments

`compilerOptions`, 주석을 제거할것인지의 설정

### noEmit

`compilerOptions`, 컴파일 에러 체크만 하고, `.js` 파일을 반환하지는 않음. 컴파일 에러가 있는지 없는지 확인만 하고 싶을 때 사용

### isolatedModules

`compilerOptions`, 각각의 파일을 다른 `module`로 변환해서 컴파일 실행하도록 하는 설정

### exclude

컴파일을 제외할 파일

### include

컴파일을 할 파일. 이외의 것들은 컴파일 되지 않음

# from lecture 1

## Implicit Types vs Explicit Types

`ts`는 `type inference (타입 추론)`을 통해 *type*을 명시적으로 표기하지 않는 경우에도 묵시적으로 정의해준다.

```typescript
let a = "hello"; // Implicit
let b: boolean = false; // explicit
```

## Types of TS

### Object

```typescript
const player: {
  name: string;
  age?: number;
} = {
  name: "nico",
};
```

### Alias Type

```typescript
type Age = number;
type Name = string;
type Player = {
  name: Name;
  age?: Age;
};

const player: Player = {
  name: "nico",
};
```

### type on function

```typescript
function playerMaker(name: string): Player {
  return {
    name,
  };
}
```

화살표 함수의 경우, 아래와 같이 작성

```typescript
const playerMaker = (name: string): Player => ({
  name,
});
```

### read-only

```typescript
type Player = {
  readonly name: Name;
  age?: Age;
};

const numbers: readonly number[] = [1, 2, 3, 4];
numbers.push(5); // error occur here
```

### Tuple

- `Tuple`은 `Array`를 생성할 수 있게 함
- 최소한의 길이를 가져야 함
- 특정 위치에 특정 타입이 있어야 함

```typescript
const player: [string, number, boolean] = ["nico", 4, true];
const playerReadOnly: readonly [string, number, boolean] = ["nico", 4, true]; // cf. readonly also available on Tuple
```

### any

값이 비어있다면 기본 타입값이 `any`임

```typescript
let a = []; // 이 경우, type은 any[] (array of any) 임
```

### unknwon

`type`을 알 수 없을 때 사용

다른 `API` 등으로 부터 데이터를 받아 왔을 때, 해당 데이터의 `type`을 모를 경우 등에 사용

이 때, `if` 문 등으로 `type`을 먼저 체크한 후, 해당 `type`에서 가능한 연산을 해주는 것은 가능

```typescript
let a: unknown;
if (typeof a === "number") {
  let b = a + 1;
}
if (typeof a === "string") {
  let b = a.toUpperCase();
}
```

### void

`void`는 아무것도 `return`하지 않는 함수에서의 반환 `type`임

보통 `void`를 명시적으로 작성해줄 필요는 없음

```typescript
function hello() {
  console.log("something");
} // return void

// same as
// function hello() : void {
//  console.log("somnething);
// }
```

### never

함수가 절대 `return` 하지 않을 경우에 사용

함수에서 `exception (예외)` 등이 발생할 때 사용

또, `type`이 두가지 일 수 도 있는 경우에도 사용

```typescript
function hello(): never {
  throw new Error("something wrong");
  // return 이 아닌 error throw 발생
}
```

```typescript
function hello(name: string | number): never {
  if (typeof name === "string") {
    name; // name의 type이 string
  } else if (typeof name === "number") {
    name; // name의 type이 number
  } else {
    name; // name의 type이 never
    // 절대 실행되지 않는 부분
  }
}
```

## functions

`typescript`에서의 `function` 에 대해 다룬 내용

### Arrrow function

```typescript
const add = (a: number, b: number) => a + b;
```

### Call Signatures

```typescript
type Add = (a: number, b: number) => number; // Call Signature

const add: Add = (a, b) => a + b;
```

또는, 다음과 같은 방법으로 작성

```typescript
type Add = {
  (a: number, b: number): number;
};

const add: Add = (a, b) => a + b;
```

### Overloading

`Overloading`은 함수가 여러 개의 `Call Signature` 를 가지고 있을 때 발생

```typescript
type Add = {
  (a: number, b: number): number;
  (a: number, b: string): number;
};

const add: Add = (a, b) => {
  if (typeof b === "string") return a;
  return a + b;
};
```

다음은 `next.js` 에서의 `Router` 에서의 `Push` 메소드에서의 `Overloading` 에시임

```typescript
type Config = {
  path: string;
  state: object;
};

type Push = {
  (path: string): void;
  (obj: Config): void;
};

const push: Push = (config) => {
  if (typeof config === "string") console.log(config);
  else {
    console.log(config.path);
  }
};
```

또 아래와 같이 `parameter` 개수에 따라서도 `Overloading` 을 사용함

```typescript
type Add = {
  (a: number, b: number): number;
  (a: number, b: number, c: number): number;
};

const add: Add = (a, b, c?: number) => {
  if (c) return a + b + c;
  return a + b;
};
```

### Polymorphism

여러 다른 구조, 여러 다른 형태를 뜻함

```typescript
type SuperPrint = {
  (arr: number[]): void;
  (arr: boolean[]): void;
  (arr: string[]): void;
};

const superPrint: SuperPrint = (arr) => {
  arr.forEach((i) => console.log(i));
};

superPrint([1, 2, 3, 4]);
superPrint([true, false, true]);
superPrint(["a", "b", "c"]);
superPrint([1, 2, true, false]); // Call Signature 가 존재하지 않기 때문에 동작하지 않음
```

위의 코드 예시와 같이 형태를 일일히 `Call Signature` 로 표현하기 보다, 아래와 같이 `generic` 을 사용할 수 있음

```typescript
type SuperPrint = {
  <T>(arr: T[]): void;
};

const superPrint: SuperPrint = (arr) => {
  arr.forEach((i) => console.log(i));
};

superPrint([1, 2, 3, 4]);
superPrint([true, false, true]);
superPrint(["a", "b", "c"]);
superPrint([1, 2, true, false]); // 모두 동작
```

즉, `generic` 을 통해 `Polymorphism` 을 구현한 것?

### Generics Recap

아래는 `generic`을 활용한 에제 코드이다

```typescript
type Player<E> = {
  name: string;
  extraInfo: E;
};
type NicoExtra = {
  favFood: string;
};
type NicoPlayer = Player<NicoExtra>;

const nico: NicoPlayer = {
  name: "nico",
  extraInfo: {
    favFood: "kimchi",
  },
};

const lynn: Player<null> = {
  name: "lynn",
  extraInfo: null,
};
```

## Classes and Interfaces

`typescript`에서의 `Class` 및 `Interface` 에 대해 다룬 내용

### Class

다음과 같이 `class` 를 선언하고 이용한다

```typescript
class Player {
  constructor(
    private firstName: string,
    private lastName: string,
    public nickname: string
  ) {}
}

const nico = new Player("nico", "las", "nc");
```

아래와 같이 `abstract class` 또한 지원한다

`abstract class` 는 다른 `class` 가 상속받을 수 있는 `class` 를 의미한다

단, `abstract class` 로는 `instance` 를 생성하지 못한다

말그대로, 추상적으로 존재하는 `class` 임

```typescript
abstract class User {
  constructor(
    private firstName: string,
    private lastName: string,
    public nickname: string
  ) {}
}

class Player extends User {}
```

아래의 코드는 `class` 내에서 `method` 를 정의하는 예제 코드

```typescript
abstract class User {
  constructor(
    private firstName: string,
    private lastName: string,
    public nickname: string
  ) {}
  getFullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}
```

또, `abstract class` 내에서는 `abstract method` 도 정의할 수 있다

이 때, `method` 의 동작을 구현해서는 안되고, `method` 의 `Call Signature` 만 정의해야한다

`abstract method` 는 `abstract class` 를 상속받는 모든 `class` 들이 구현을 해야하는 `method` 를 의미한다

```typescript
abstract class User {
  constructor(
    private firstName: string,
    private lastName: string,
    protected nickname: string
  ) {}
  // abastact getNickName(arg : string): void
  abastact getNickName(): void
  getFullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

class Player extends User {
  getNickName (){ // 추상 메소드 이므로, 상속 받은 클래스 내에서 구현해야한다
    return this.nickname;
  }
}
```

아래 부터는 `class` 를 활용하여 `HashMap` (`dictionary` 와 유사) 을 구현하는 에제이다

```typescript
type Words = {
  [key: string]: string; // property의 이름은 모르고, type 을 알 때, [keyName : type] 과 같은 형식으로 정의
};

class Dict {
  private words: Words;
  constructor() {
    this.words = {};
  }
  add(word: Word) {
    // class 또한 type 처럼 사용 가능
    if (this.words[word.term] === undefined) {
      this.words[word.term] = word.def;
    }
  }
  def(term: string) {
    return this.words[term];
  }
}

class Word {
  constructor(public readonly term: string, public readonly def: string) {}
}

const kimchi = new Word("kimchi", "korean food");

const dict = new Dict();

dict.add(kimchi);
dict.def("kimchi");
```

### Interface

`type`에 비해 `Interface` 는 특히 오브젝트의 형태를 특정해주기 위한 용도로만 사용

`Interface` 또한 `type` 으로 사용할 수 있음

오브젝트의 형태를 특정하는 법에는

`type` 또는 `Interface` 두 가지 방법이 있으나, `type`의 경우에는 주로 타입 표기를 위해 사용함

`Interface` 는 `extends` 를 통해 쉽게 상속이 가능하지만, `type`의 경우에는 상속을 `&` 연산자로 표기해야함

```typescript
type team = "red" | "yellow" | "blue";
type health = 0 | 5 | 10;
type Player = {
  nickname: string;
  team: Team;
  health: Health;
};

interface Player {
  nickname: string;
  team: Team;
  health: Health;
}

const nico: Player = {
  nickname: "nico",
  team: "yellow",
  health: 10,
};
```

```typescript
interface User {
  name: string;
}

interface Player extends User {}

const nico: Player = {
  name: "nico",
};

type User = {
  name: string;
};
type Player = User & {};

const nico: Player = {
  name: "nico",
};
```

또, `interface` 를 통해 `abstract class`의 구현을 대체할 수 있다

이 때 해당 `interface`를 이용하는 `class` 는 `implements` 키워드로 구현한다

이 경우, `abstract class` 를 사용하는 것보다 컴파일 결과물인 `.js` 코드가 상대적으로 가벼워진다

`implements` 는 `js` 에서는 지원하지 않기 때문에, 간결한 코드로 컴파일 되기 때문이다.

```typescript
interface User {
  firstName: string;
  lastName: string;
  sayHi(name: string): string;
  fullName(): string;
}

class Player implements User {
  constructor(public firstName: string, public lastName: string) {}
  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
  sayHi(name: string) {
    return `Hello ${name}. My name is ${this.fullName()}`;
  }
}
```

### type vs interface

- `type` 을 사용하고 싶다면, 말그대로 `type` 키워드를 사용
- `type` 을 재활용 및 연장하여 상속하고 싶다면, `&` 키워드를 사용
- `interface` 를 재활용 및 연장하여 상속하고 싶다면, `extends` 키워드를 사용
- `interface` 의 명칭이 같다면, 해당 이름을 가진 `interface`는 여러 개의 속성이 자동으로 통합 됨

### Polymorphism

- `Polymorphism (다형성)` 은 다른 형태 및 모양의 코드를 가질 수 있도록 해줌
- `Polymorphism` 을 이루기 위해서, `generic` 을 사용함
- `generic` 은 `placeholder` 타입을 사용할 수 있도록 해줌
- `placeholder` 타입이란, `concrete` 타입과는 상반되는 개념
- `compile`이 실행되면, 컴파일러가 `placeholder` 타입을 `concrete` 타입으로 바꾸어줌

이하의 코드에서는 `Polymorphism`, `generic`, `class`, `interface` 등을 활용하여 브라우저에서 이용되는 `localstorage` 를 구현해보도록 함

```typescript
interface SStorage<T> {
  [key: string]: T;
}

class LocalStorage<T> {
  private storage: SStorage<T> = {};
  set(key: string, value: T) {
    this.storage[key] = value;
  }
  remove(key: string) {
    delete this.storage[key];
  }
  get(key: string): T {
    return this.storage[key];
  }
  clear() {
    this.storage = {};
  }
}

const stringsStorage = new LocalStorage<string>();
stringStorage.get("key");

const booleanStorage = new LocalStorage<boolean>();
booleanStorage.get("xxx");
```

## TS Settings

`Nest.js`, `Next.js`, `Create-React-App` 등 을 사용하는 대부분의 경우에는 수동으로 `typescript` 프로젝트를 설정할 기회가 거의 없다

이에, `ts` 설정법을 따로 이해해두는 편이 알고 쓰고자하는 방향성에는 부합

(`webpack` 또한 직접 설정할 일이 잘 없는 것과 유사)

### Install TS

```bash
npm i -D typescript
```

`-D` 옵션을 통해, `ts`가 `devDependencies` 에 설치되도록 함

### Set tsconfig.json

프로젝트 최상위 경로에 `tsconfig.json` 파일을 생성한다

```bash
touch tsconfig.json
```

이후 `tsconfig.json` 파일 내에 아래와 같이 작성

```json
{
  "include": ["src"],
  "compilerOptions": { "outDir": "build" }
}
```

`include` 옵션은, `compile` 대상이 되는 파일이 되는 경로를 작성

`compilerOptions` 옵션에서 `outDir` 속성은, 컴파일의 결과물인 `.js` 파일의 출력 경로를 작성

이후 `package.json` 파일에서, `scripts` 에 다음과 같이 작성

```json
{
  "scripts": {
    "build": "tsc"
  }
}
```

위의 작업 후, `terminal` 에서 `npm run build` 혹은 `tsc` 명령어를 입력하면 컴파일의 결과물이 설정한 경로에 생성된다

```json
{
  "scripts": {
    "build": "tsc",
    "start": "node build/index.js"
  }
}
```

이와 함께 위와 같이 `start`를 정의하면, 실행하도록 설정해줄 수도 있다

결국 `npm run build && npm run start` 와 같이 동작해야 하는데,

이를 용이하게 하기 위해 `ts-node`를 설치한다

```bash
npm i -D ts-node
```

`ts-node`는 빌드 없이 `.ts` 를 실행할 수 있게 해주는 프로덕션용이 아닌 개발 환경에서만 사용하는 패키지이다

설치 후, 아래와 같이 `dev` 를 정의해준다

```json
{
  "scripts": {
    "build": "tsc",
    "start": "node build/index.js",
    "dev": "ts-node src/index"
  }
}
```

이후, 코드가 수정될 때마다 실행되게 설정하고 싶다면, `nodemon` 등을 활용한다

다음으로, `comilerOptions` 에서 `outDir` 외의 여러 속성을 설정 하여 컴파일 옵션을 바꿔줄 수 있다

가령, 아래와 같이 `target` 필드를 사용하면 해당 `JavaScript` 표준으로 컴파일 해준다

```json
{
  "include": ["src"],
  "compilerOptions": { "outDir": "build", "target": "ES6" }
}
```

일반적으로 `ES5` 또는 `ES6` 를 이용하게 되는데, 이는 대부분의 `node.js` 버전과 대부분의 `browser` 가 현재 지원하고 있기 때문이다

다음으로 알아볼 `lib` 속성은, 합쳐진 라이브러리의 `declaration file (정의 파일)`을 특정해주는 역할을 함

즉, `lib` 속성은 `JavaScript`의 어떤 버전이 그 환경에서 사용되는지를 말함

작성할 코드가 `ES6` 를 지원하는 환경에서 실행될 것이라고 지정하고, 또 코드가 `Browser` 에서 동작할 것이라면 아래와 같이 작성

```json
{
  "include": ["src"],
  "compilerOptions": { "outDir": "build", "target": "ES6" },
  "lib": ["ES6", "DOM"]
}
```

가령, `lib` 속성에 `DOM` 을 추가해주면, `TypeScript` 파일을 작성할 때, `document.` 을 입력했을 때 `document` 와 관련된 여러 메소드 및 인터페이스를 `vs code`에서 자동 제안함

예를 들어, `document.qu` 까지 작성한 경우, `document.querySelector` 를 자동 완성해주는 등, `ts` 자체적으로 라이브러리를 미리 구현해두었고,

이를 지원해준다는 측면에서 해당 필드명이 `lib` 임

### Declaration Files

앞서 `tsconfig.json` 에서 언급한 `declaration file (정의 파일)` 는 `built-in JS APIs` 라고 칭할 수 있다

즉, `JavaScript` 에서의 `Math`, `Document`, `Map` 등을 정의해둔 파일을 말한다

이러한 `declaration file (정의 파일)` 은 `.d.ts` 확장자로 정의된다

가령, `myPacakge.js` 가 `npm` 에서 공유되는 `module` 이라 가정하고 아래와 같은 코드로 정의되어있다고 하자

```javascript
export function init(config) {
  return true;
}

export function exit(code) {
  return code + 1;
}
```

두 개의 _init_, _exit_ 함수로 구현되어있고, 이를 아래와 같이 `.ts` 파일에서 불러오면,

```typescript
import { init, exit } from "myPackage";
```

_init_ 함수에 대한 `Call Signature` 등이 정의 되어 있지 않기 때문에 오류가 발생한다

이에, 아래와 같은 코드로 `myPackage.d.ts` 의 이름으로 `declaration file` 을 작성해야 한다

```typescript
interface Config {
  url: string;
}
declare module "myPackage" {
  function init(config: Config): boolean;
  function exit(code: number): number;
}
```

`declaration file` 을 작성한 후에는 더 이상 `.ts` 파일에서 오류를 발생시키지 않는다

실제로 `.d.ts` 의 `declaration file` 을 작성할 일은 없는 편이지만, 마찬가지로 이해를 하고 사용하는 것이 중요한데,

`JavaScript` 로 작성된 많은 라이브러리 및 모듈들이 이와 같은 방식으로 `TypeScript` 에서 구동되기 때문이다

### Using JavaScript File on TypeScript File

`JavaScript` 파일을 `TypeScript` 파일에 `import` 해서 사용하는 법에 대해 알아본다

이 경우, `TypeScript` 파일에서 아래와 같이 `import` 시에 경로명에 `./` 을 붙여준다

```typescript
import { init, exit } from "./myPackage";
```

이후 `tsconfig.json` 에서 `compilerOptions` 이하에 아래와 같은 속성을 추가한다

```json
{
  "compilerOptions": {
    "allowJs": true
  }
}
```

이 경우, `any` 타입 등으로 `TypeScript` 가 `import` 한 `JavaScript` 의 `Call Signature` 를 추론하여 동작한다

기존 `JavaScript` 프로젝트를 `TypeScript` 로 서서히 전환해갈 때 등, 두 파일이 섞여서 진행되는 경우가 있는데,

이 경우, `JavaScript` 파일에서 아래와 같은 주석(`// @ts-check `)을 최상단 첫째줄에 작성해주면, `JavaScript` 또한 `TypeScript` 컴파일러가 체크해준다

작성은 `.js` 로 하되, 체크만 해주는 것이다

```javascript
// @ts-check

//... js codes
```

또, `JSDoc` 을 이용하면, 주석만으로 `.js` 파일에서 `.ts` 와 같이 타입 체크를 하며 작성할 수 있다

### DefinitelyTyped

`npm` 의 여러 모듈 중에 `TypeScript` 의 지원을 할 수 있도록 구현해둔 것이 `definitelyTyped` 이다

이를 사용하기 위해서 해당 모듈이 `definitelyTyped` 에서 지원하고 아래와 같이 `@types/모듈이름` 의 형식으로 설치한다

```bash
npm i -D @types/node
```

`@types/node` 의 경우 `node.js` 에서 사용되는 기본 API 들에 대한 타입정의가 되어있다

최근에는 모듈자체를 설치하면 `.d.ts` 파일이 함께 설치되어 이미 타입 정의가 잘 된 경우도 많아졌다

## References

[tsconfig.json Property 알아보기](https://it-eldorado.tistory.com/128){: target="\_blank"}

[tsconfig.json Property 공식 Document](https://aka.ms/tsconfig.json){: target="\_blank"}
