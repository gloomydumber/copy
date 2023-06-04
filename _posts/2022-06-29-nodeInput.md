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

| ![keyboardInput](/assets/posts/2022-06-29-nodeInput/keyboardInput.gif) |
| :--------------------------------------------------------------------: |
|                        <b>Input & Output...</b>                        |

본 포스팅에서는 *Node.js*에서 *데이터 및 파일 입출력*을 처리 하는 방법을 알아본다.

*JavaScript*로 *PS* 등을 하는 경우에도 참고하여 이용

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

### PS의 경우

```js
// 입력값이 한 줄일 경우
let input = require('fs').readFileSync(filePath).toString().trim();
```

```js
// 입력값이 여러 줄일 경우
let input = require('fs').readFileSync(filePath).toString().trim().split(' ');
```

### readline

_readline_ module은 *Node.js*의 기본 내장 모듈이다.

### readline / 한 줄 입력받기

```javascript
const readline = require("readline");

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

rl.on("line", (line) => {
  // 한 줄 입력 후 실행 되는 영역
  console.log("you typed:", line);
  rl.close(); // close를 표기하지 않으면 무한히 입력 받음
});

rl.on("close", () => {
  // 입력 종료시 실행 됨
  process.exit();
});
```

### fs

*fs*는 *File System*의 약자로, 마찬가지로 *Node.js*의 기본 내장 모듈이다

### fs / readFile()

```javascript
fs.readFile(filename, [options], callback);
```

파일을 *asynchronous (비동기적)*으로 읽는다.

### fs / readFileSync()

```javascript
fs.readFileSync(filename, [options]);
```

*readFile()*과는 달리, 파일을 *synchronous (동기적)*으로 읽는다.

## ✔️ process.stdin

```javascript
async function userEnterInputForExecutionOnError() {
  while (true) {
    try {
      console.log("execute instructions here");
      throw "intentional error";
    } catch (error) {
      console.log("Press enter to try again...");
      await new Promise(function (resolve, reject) {
        process.stdin.resume();
        process.stdin.once("data", function (data) {
          process.stdin.pause();
          resolve();
        });
      });
    }
  }
}

userEnterInputForExecutionOnError();
```

반복적인 비동기적 요청 등을 할 때, 오류가 발생했을 때 터미널을 통해 해당 요청을 다시 시도할 것인의 여부를 _enter_ 입력을 통해 받아들이는 코드 예제

_Promise_ 객체와 *process.stdin*을 활용하였다.

*process.stdin*에 관한 자세한 명세 파악 확인 필요

<!-- ## References -->

<!-- [Array on mozzila.org](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array){: target="\_blank"} -->

<!-- ![permasecond](/assets/posts/2020-02-21-cmdcolor/permasecond.png) -->
