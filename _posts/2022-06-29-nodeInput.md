---
layout: post
title: "JavaScript / Node.js 입출력 처리하기"
date: 2022-06-29 09:00:00
categories: WEB-BACKEND
permalink: /archivers/nodeInput
nocomments: false
use_math: true
---

# JavaScript / Node.js 입출력 처리하기

![keyboardInput](/assets/posts/2022-06-29-nodeInput/keyboardInput.gif)
|:--:|
| <b>Keyboard Input</b>|

본 포스팅에서는 *Node.js*에서 *데이터 및 파일 입출력*을 처리 하는 방법을 알아본다.

## 출력

출력의 경우, 기본적으로 *console.log*를 이용한다.

### methods of console

_console.log_ 이외에, _info_, _warning_, _debug_, _error_ 등을 활용할 수 있다.

```javascript
console.log("msg":);
console.info("info");
console.warn("warning");
console.debug("debug");
console.error("error");
```

*chrome*에서 *console.debug*로 출력한 내용이 나오지 않을 수 있는데, 콘솔 출력 옵션을 확인해주어야 한다.

```javascript
console.log(document);
console.dir(document);
```

*console.log*의 경우 _HTML_ 형식의 _Tree_ 구조로 출력하고, *console.dir*의 경우 _JSON_ 형식의 _Tree_ 구조로 출력한다.

```javascript
console.group("personal infomation");
console.log("name is caesar.");
console.log("age is 30.");
console.groupEnd();
```

*console.group*을 활용하여, 여러 *console.log*를 묶어서 출력할 수 있다.

```javascript
const userStatusArray = ["caesar", 30];
const userStatusObject = { name: "caesar", age: 30 };
console.table(userStatusArray);
console.table(userStatusObject);
```

*Array*나 _Oject_ 타입을 *console.table*로 출력할 수 있다.

```javascript
console.count("DO NOT CLICK THIS BUTTON");
console.count("DO NOT CLICK THIS BUTTON");
console.count("DO NOT CLICK THIS BUTTON");
console.count("DO NOT CLICK THIS BUTTON");
console.countReset("DO NOT CLICK THIS BUTTON");
```

*console.count*로 호출된 횟수를 출력하고, *console.countReset*으로 초기화 할 수 있다.

```javascript
console.time("timer");
let someDifficultCalculation = 0;
for (let i = 0; i < 500; i++) {
  someDifficultCalculation++;
}
console.timeEnd("timer");
```

_console.time_ 및 *console.timeEnd*를 활용하여, 코드가 연산 되는데에 소요되는 시간을 측정할 수 있다.

## console.log examples

```javascript
const name = "caesar";
const age = 30;

console.log(name, age);
console.log("%s is %d years old.", name, age);
console.log(`${name} is ${age} years old.`);
```

## style on console.log

```javascript
console.log(
  "%cFont Colour and Font Size are changed.",
  "color: lime; font-size: 20px;"
);
console.log("%cYin %cand %cYang", "color: red", "color: black", "color: blue");
```

## 입력

### readline

_readline_ module은 ~

## References

<!-- [Array on mozzila.org](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array){: target="\_blank"} -->

<!-- ![permasecond](/assets/posts/2020-02-21-cmdcolor/permasecond.png) -->
