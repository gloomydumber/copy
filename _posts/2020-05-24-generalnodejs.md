---
layout: post
title: "Node.js"
date: 2020-05-24 12:40:00
categories: WEB-BACKEND
permalink: /archivers/nodejs
nocomments: false
use_math: true
---

# Node.js

Node.js에 관해 공부하며 간단 메모한 글

## What is Node.js?

what is Node.js

## URL

![url](/assets/posts/2020-05-24-nodejs/url.png)

[URL structure (클릭 시 새 탭 열기)](https://howurls.work/#/){: target="\_blank"}

default port = 80

## NPM

Node.js Pacakge Manager

## 문법 간단 메모

### Template Literal

```js
var name = "leaveroov";
var letter = `Dear ${name}

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. ${name} Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. ${
  1 + 1
} Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa egoing qui officia deserunt mollit anim id est laborum. ${name}`;

console.log(letter);
```

### CRUD

Create & Read & Update & Delete

### synchronous & asynchronous

동기 / 비동기

<!-- ![executeCMD](/assets/posts/2020-03-21-bithumbcal/bithumbcal.gif)
 [PHP 표준 Library (클릭 시 새 탭 열기)](https://www.php.net/manual/en/){: target="\_blank"}
 [PHP type table Document (클릭 시 새 탭 열기)](http://php.net/manual/en/types.comparisons.php){: target="\_blank"}
 -->

## 필기노트

### Express 두 번 호출 현상

[2번 호출 현상](https://medium.com/sjk5766/node-express-api%EA%B0%80-%EB%91%90-%EB%B2%88-%ED%98%B8%EC%B6%9C%EB%90%98%EB%8A%94-%ED%98%84%EC%83%81-b11f98a064e){: target="\_blank"}

### 정적파일 호출

[express에서 static file 호출](https://m.blog.naver.com/PostView.nhn?blogId=pjok1122&logNo=221545195520&proxyReferer=https:%2F%2Fwww.google.com%2F){: target="\_blank"}

### WINDOWS Node.js Update

https://jamong-icetea.tistory.com/355 npm install -g n 지원안함

### node webkit으로 Web App .exe 만들기

강좌 1 (기본) https://web-inf.tistory.com/28

강좌 2 (exe 단일화) https://web-inf.tistory.com/29

아이콘 변경 https://gyuha.tistory.com/473

우클릭막기 https://stackoverflow.com/questions/737022/how-do-i-disable-right-click-on-my-web-page

F12막기 https://stackoverflow.com/questions/28575722/how-can-i-block-f12-keyboard-key-in-jquery-for-all-my-pages-and-elements
