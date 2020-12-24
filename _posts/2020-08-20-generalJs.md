---
layout: post
title: "JavaScript"
date: 2020-08-20 10:00:00
categories: WEB-FRONTEND
permalink: /archivers/Javascript
nocomments: false
use_math: true
---

# JavaScript

JavaScript에 관해 공부하며 간단 메모한 글

https://ko.javascript.info/?utm_source=Nomad+Academy&utm_campaign=8026b5711e-EMAIL_CAMPAIGN_2020_12_09_07_02_COPY_01&utm_medium=email&utm_term=0_4313d957c9-8026b5711e-155724345 모던 자바스크립트 총 정리

## What is JavaScript?

[JavaScript 언어 동작 방식 1 (클릭 시 새 탭 열기)](https://doitnow-man.tistory.com/128){: target="\_blank"}

[JavaScript 언어 동작 방식 2 (클릭 시 새 탭 열기)](https://engineering.huiseoul.com/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%9E%91%EB%8F%99%ED%95%98%EB%8A%94%EA%B0%80-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84%EC%99%80-%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%98-%EB%B6%80%EC%83%81-async-await%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%BD%94%EB%94%A9-%ED%8C%81-%EB%8B%A4%EC%84%AF-%EA%B0%80%EC%A7%80-df65ffb4e7e){: target="\_blank"}

[JavaScript 코드 최적화 하기 (클릭 시 새 탭 열기)](https://mollangk.tistory.com/14){: target="\_blank"}

[JavaScript 비동기 처리 로직 (클릭 시 새 탭 열기)](https://medium.com/@nsh235482/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%97%90%EC%84%9C%EC%9D%98-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%B2%98%EB%A6%AC-%EB%A1%9C%EC%A7%81-93a09e96dc36){: target="\_blank"}

[JavaScript 코드 동작 순서 (클릭 시 새 탭 열기)](https://medium.com/gdana/%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%98%B8%EC%B6%9C%EC%88%9C%EC%84%9C-d51a4349be71){: target="\_blank"}

[JavaScript 동기/비동기로 매끄럽게하기 \*\*\*! (클릭 시 새 탭 열기)](https://engineering.linecorp.com/ko/blog/dont-block-the-event-loop/){: target="\_blank"}

<!-- javascript구조에 관한 사진 -->

## hoisting

기본적으로 JavaScript는 Synchronous Script

var, function declaration 등의 변수, 함수 선언문은 script 최상단으로 위동 되어 실행 됨

## General coding rule

'use strict'

var hoisting

avoid nested loop

early return, early exit (if조건문 사용시 조건에 안맞을 시 빨리 return 후 필요한 logic 작성 등)

function expression / function declaration 구분

arrow function

IIFE(Immeadiately Invoked Functgion Expression)

class(template), object(instance of class), OOP(Object Oriented Programming)

[화살표 함수](https://velog.io/@parksj3205/2019-08-30-1208-%EC%9E%91%EC%84%B1%EB%90%A8){: target="\_blank"}

## script Attribute

[async, defer 이미지(클릭 시 새 탭 열기)](https://appletree.or.kr/blog/web-development/javascript/script-%ED%83%9C%EA%B7%B8%EC%9D%98-async%EC%99%80-defer-%EC%86%8D%EC%84%B1/){: target="\_blank"}

<!-- async defer 동작 이미지 사진 -->

## AJAX

<!-- ![url](/assets/posts/2020-05-24-nodejs/url.png) -->

[XMLHttpRequest에 관하여 - zerocho (클릭 시 새 탭 열기)](https://www.zerocho.com/category/HTML&DOM/post/594bc4e9991b0e0018fff5ed){: target="\_blank"}

[ajax 데이터 로딩 중 로딩 표시(클릭 시 새 탭 열기)](https://skylhs3.tistory.com/4){: target="\_blank"}

[ajax 속성 등(클릭 시 새 탭 열기)](https://tutorialpost.apptilus.com/code/posts/jquery/jq-ajax/){: target="\_blank"}

[ajax CORS 문제에 관하여(클릭 시 새 탭 열기)](https://infotake.tistory.com/88){: target="\_blank"}

[ajax CORS 문제 nodeJS로 해결 (클릭 시 새 탭 열기)](https://kosaf04pyh.tistory.com/25){: target="\_blank"}

[하나은행 환율 API 간헐적 CORS/CORB (클릭 시 새 탭 열기)](https://blog.edit.kr/entry/PHP-%ED%95%98%EB%82%98-%EC%9D%80%ED%96%89-%ED%99%98%EC%9C%A8-API%EB%A5%BC-%ED%86%B5%ED%95%9C-JSON){: target="\_blank"}

[간헐적 CORS 사례 (클릭 시 새 탭 열기)](https://devtalk.kakao.com/t/cors/104823/5){: target="\_blank"}

[xmlhttprequest인자로 비동기/동기 (클릭 시 새 탭 열기)](https://funylife.tistory.com/entry/XMLHttpRequest-%EA%B0%9D%EC%B2%B4-%EB%8F%99%EA%B8%B0%EB%B0%A9%EC%8B%9D-%EB%B9%84%EB%8F%99%EA%B8%B0%EB%B0%A9%EC%8B%9D%EC%9D%98-%EC%8B%A4%ED%96%89%EC%B0%A8%EC%9D%B4){: target="\_blank"}

[xmlhttpreques시 try catch로 오류판단(클릭 시 새 탭 열기)](https://stackoverflow.com/questions/5018566/catching-xmlhttprequest-cross-domain-errors){: target="\_blank"}

[CORS error catch 가능여부](https://stackoverflow.com/questions/42721584/catching-an-access-control-allow-origin-error-in-javascript){: target="\_blank"}

[CORS error catch 가능여부 2 (보안문제로 불가)](https://stackoverflow.com/questions/19325314/how-to-detect-cross-origin-cors-error-vs-other-types-of-errors-for-xmlhttpreq){: target="\_blank"}

HTTP Request 할 시, setRequestHeader를 통해, Header설정 하는 방법 등

script generated HTML code 를 parsing 하는 방법 (only JS?)

http -> https 요청 (가능) / https -> http 요청 (불가) ->> hosting받아서 http로 만듦...

[/wapi/v3/assetDetail.html API (클릭 시 새 탭 열기)](https://github.com/binance-exchange/binance-official-api-docs/blob/master/wapi-api.md){: target="\_blank"}

## setTimeOut

[for문 안 에서의 setTimeOut (클릭 시 새 탭 열기)](http://yoonbumtae.com/?p=643){: target="\_blank"}

## jQuery

[jQuery에서 append() 사용 시 주의 할 점 (클릭 시 새 탭 열기)](https://jonnung.tistory.com/16){: target="\_blank"}

[input 값 변화 실시간 감지(propertychange change keyup paste input)](https://okky.kr/article/686735){: target="\_blank"}

[script src의 위치와 \$(document).ready/window.onload 상관관계](https://hellogk.tistory.com/18){: target="\_blank"}

[script src의 위치와 \$(document).ready/window.onload 상관관계 2](https://dololak.tistory.com/369){: target="\_blank"}

[$(function(){})의 의미](https://mine-it-record.tistory.com/288){: target="\_blank"}

## async & await

[await 병렬 처리](https://www.youtube.com/watch?v=aoQSOZfz3vQ&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=13){: target="\_blank"}

## getbyElement, querySelector

[geybyElement, querySelector 차이](https://humahumahuma.tistory.com/122){: target="\_blank"}

[geybyElement, querySelector 차이 2](https://blog.eunsatio.io/develop/Javascript%EB%A1%9C-HTML-%EC%9A%94%EC%86%8C-%EC%88%9C%ED%9A%8C%ED%95%98%EA%B8%B0){: target="\_blank"}

## form submit without Redirection

이벤트리스너로 preventDefault() 설정해주기

```js
const ajaxtest = document.getElementById("ajaxLogin");
ajaxtest.addEventListener("submit", function (evt) {
  evt.preventDefault();
  login();
});
```

## etc

### effect

[특정 수치 이상일 경우 배경색 변경(table cell의 경우) (클릭 시 새 탭 열기)](https://www.codeproject.com/Questions/1202518/Blink-td-cells-background-in-the-row-if-value-is-g){: target="\_blank"}

[notification 효과 주는 JS (클릭 시 새 탭 열기)](https://alertifyjs.com/notifier/message.html){: target="\_blank"}

[이벤트 버블링, 캡쳐, 위임](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/){: target="\_blank"}

### 숫자 처리

개발중, toFixed를 7주고 함수 만들어서 그 함수에 전달 후, 10000이상이면 tofixed2 등 처리 할 생각이었으나, \*1 하는 방법 사용

소수인 number 변수에 \* 1 을 하면 끝 0을 제외한 유효숫자 까지만 표기 됨

(ex : var a = 0.5600 인경우 console.log(a\*1) 은 0.56으로 표기 됨)

### forced reflow while executing JavaSCript took Xms

빈번한 DOM 구조 변경으로 인한 에러

[관련링크 (클릭 시 새 탭 열기)](https://www.facebook.com/groups/codingeverybody/permalink/2210757938964730/){: target="\_blank"}

tippy.js 적용시 발생.. 빈번한 DOM 구조 변경인듯

### XMLHttpRequest 등으로 외부 요청 시 cache되어 데이터가 update되지 않는 문제

https://stackoverflow.com/questions/6090990/how-can-i-avoid-cached-data-when-downloading-via-xmlhttprequest

time을 이용

https://stackoverflow.com/questions/168963/stop-jquery-load-response-from-being-cached/168977#168977

ajax 에서 no cache option

### vanilla JS

[jquery이전에 배웠으면 좋았을 것들](https://jeonghwan-kim.github.io/2018/01/25/before-jquery.html){: target="\_blank"}

### PM2

[pm2 사용법, backend 로 옮길 것](https://engineering.linecorp.com/ko/blog/pm2-nodejs/){: target="\_blank"}

### Promise.All

promise.all 에 관하여..

promise.AllSettled 에 관하여 ...

## DOM 노드 생성 및 추가

[DOM 노드 생성 및 추가 \*\*](https://antaehyeon.github.io/devlog/2018/07/13/%EC%BD%94%EB%93%9C%EC%8A%A4%EC%BF%BC%EB%93%9C-DOM-node-%EC%83%9D%EC%84%B1-%EB%B0%8F-%EC%B6%94%EA%B0%80/){: target="\_blank"}

## 문법 간단 메모

### array

array에서 indexof 사용 시, 원하는 값 편리하게 찾을 수 있음

```js
var name = "foo, need mod";
var letter = `Dear ${name}

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. ${name} Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. ${
  1 + 1
} Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa egoing qui officia deserunt mollit anim id est laborum. ${name}`;

console.log(letter);
```

### table

[table 정렬하기 1 (클릭 시 새 탭 열기)](https://tonks.tistory.com/79){: target="\_blank"}

[table Header 클릭 시 정렬되는 예제 코드 (클릭 시 새 탭 열기)](https://m.blog.naver.com/PostView.nhn?blogId=websearch&logNo=220897790755&proxyReferer=https:%2F%2Fwww.google.com%2F){: target="\_blank"}

[table안에 이미지 딱 맞춰 넣기 (클릭 시 새 탭 열기)](https://namubada.net/203){: target="\_blank"}

### MutationObserver

[Mutation Observer 에 관하여 1 (클릭 시 새 탭 열기)](https://www.zerocho.com/category/HTML&DOM/post/5be24eacdb0c31001c4c5040){: target="\_blank"}

[Mutation Observer 에 관하여 2 (클릭 시 새 탭 열기)](https://uxgjs.tistory.com/170){: target="\_blank"}

[Mutation Observer Stackoverflow my question (클릭 시 새 탭 열기)](https://stackoverflow.com/questions/63483340/detect-change-of-textcontentvalue-of-html-table-cell-and-apply-animation-on-ch){: target="\_blank"}

(Mutation oberver에서 config를 subtree: true로 설정하라 함)

### localStorage

[클라이언트에 정보, 데이터 저장 (클릭 시 새 탭 열기)](http://bitly.kr/w362h3jwQ1Z){: target="\_blank"}

[localStroage에 배열형식 value 저장](https://amajoy.tistory.com/entry/localStorage-%EB%B0%B0%EC%97%B4%ED%98%95%EC%8B%9D-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B8%B0){: target="\_blank"}

### input

[input에서 enter키 누를 시 이벤트 발생 (클릭 시 새 탭 열기)](https://hsol.tistory.com/550){: target="\_blank"}

### 반복문

[for loop 최적화](https://niceman.tistory.com/27){: target="\_blank"}

### Date() 객체

[브라우저 마다 다른 Date()객체 처리 방식](https://meetup.toast.com/posts/130){: target="\_blank"}

### JS 암호화 라이브러리

[JS Encrypt](https://offbyone.tistory.com/343){: target="\_blank"}

### 즉시 실행 SetInterval의 hack

https://stackoverflow.com/questions/6685396/execute-the-setinterval-function-without-delay-the-first-time
