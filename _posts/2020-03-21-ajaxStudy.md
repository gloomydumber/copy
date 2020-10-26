---
layout: post
title: "Ajax"
date: 2020-03-21 03:06:00
categories: WEB-FRONTEND
permalink: /archivers/ajax
nocomments: false
use_math: true
---

# Ajax

Ajax(Asynchronous JavaScript and XML)는 자바스크립트를 이용해서 비동기적(Asynchronous)으로 서버와 브라우저가 데이터를 교환할 수 있는 통신 방식을 의미한다.

웹페이지의 페이지 전체를 다시 리로드하는 동기적 방식이 아닌, 페이지의 일부분을 비동기적으로 통신하는 방법.

하나의 페이지에서 여러 정보를 표현할 수 있는 방법을 Single Page Application 이라 하고, Ajax는 이를 달성하기 위한 하나의 수단.

(SPA의 예로는, 내 2005-06년 기억 속의 http://suso.ba.ro)

### Fetch API

Ajax 구현을 위한 최신 방법

https://opentutorials.org/course/3281/20565

https://gs.saro.me/dev?page=6&tn=564

POST FETCH 참고 https://hogni.tistory.com/142

fetch 유튜브 in 6minutes https://www.youtube.com/watch?v=cuEtnrL9-H0

fetch의 기반인 promise에 대한 youtube https://www.youtube.com/watch?v=JB_yU6Oe2eE

```html
<html>
  <body>
    <input
      type="button"
      value="click it"
      onclick="function callbackme(){console.log('response end');} fetch('경로 및 이름').then(callbackme); console.log(1); console.log(2);"
    />
    <!-- Asynchronous.. fetch의 요청이 끝나면,(then,) callbackme 함수를 실행하라.  -->
    <!-- 결국 fetch 요청을 server로 보낸 뒤 응답이 오기전에 console.log(1) 및 consoloe.log(2) 등을 수행하고 있다가 응답이 오면 그에 따라 실행-->
    <!-- 아래는 함수 익명화 -->
    <input
      type="button"
      value="click it(익명함수화)"
      onclick="fetch('경로 및 이름').then(function(){console.log('response end');}); console.log(1); console.log(2);"
    />
  </body>
</html>
```

다음과 같이 콜백 함수에 response 객체를 줄 수 있다.

```html
<html>
  <body>
    <input
      type="button"
      value="click it(익명함수화)"
      onclick="fetch('경로 및 이름').then(function(response){console.log(response);}); console.log(1); console.log(2);"
    />
    <!-- response.status를 console.log로 호출하면 200, 404 등의 값만 불러올 수 있다. -->
  </body>
</html>
```

Response 객체의 멤버, 속성 값
Status 200 : 정상
Status 404 : Not Found

이를 활용하여 다음과 같이 조건문을 이용해 서버에서 데이터를 제대로 불러왔는지 검사할 수 있다.

```html
<html>
  <body>
    <input
      type="button"
      value="click it(익명함수화)"
      onclick="fetch('경로 및 이름').then(function(response){if(response.status == '404'){alert('Not Found');};}); console.log(1); console.log(2);"
    />
  </body>
</html>
```
