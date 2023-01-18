---
layout: post
title: "rxjs AJAX and WebSocket on Node.js"
date: 2022-08-03T00:00:10Z
categories: ReactiveX
permalink: /archivers/rxjsAjaxWsNode
nocomments: false
use_math: true
published: false
---

# published false 지우고 작성시작해야함 // 📙📙📙📙📙📙📙📙📙📙📙📙📙📙📙📙📙📙📙📙📙📙

# retry vs repeat Operator

| ![reactivex](/assets/posts/2022-08-02-nodeRxjsAjaxAndWS/reactivex_logo.png) |
| :-------------------------------------------------------------------------: |
|                                   <b></b>                                   |

## WebSocket auto-reconnect on rxjs

이 포스트는, `rxjs`를 활용한 `Node.js` 프로젝트에서, `ws` 모듈을 이용한 `WebSocket`의 연결 해제 시에 어떻게 하면 자동으로 다시 재연결을 시도하도록 구현 할 수 있는지에 대해 알아보다 작성하게 되었다.

이에, 본격적으로 `AJAX` 및 `W`

## AJAX | WebSocket of rxjs in Node.js

우선, `rxjs`는 본래 _browser_ 환경을 위해 구현된 라이브러리 이므로, `Node.js` 환경에서 `AJAX` 또는 `WebSocket`을 이용하기 위해서는 다음과 같은 설정이 필요하다.

`AJAX`를 이용하기 위해서는, `xhr2` 모듈을 `rxjs` 라이브러리에 정의 된 `XMLHttpRequest` 객체를 replace해야한다.

```javascript
global.XMLHttpRequest = require("xhr2"); // for Server Side Ajax
const { ajax } = require("rxjs/ajax");

const obs$ = ajax("https://api.github.com/users?per_page=5");
obs$.subscribe((result) => console.log(result.response));
```

`WebSocket`의 경우도 마찬가지로, `ws` 모듈을 `rxjs` 라이브러리에 정의 된 `WebSocket` 객체를 replace 해주고 난 다음 사용한다.

```javascript
global.WebSocket = require("ws");
const { webSocket } = require("rxjs/webSocket");
const webSocketSubject = webSocket("ws://localhost:8001");
webSocketSubject.subscribe(console.log);
```

## TL;DR

`retry`는 `error` 시에 동작

`repeat`은 `complete` 시에 동작

## References

[🔗 rxjs/ajax on Node.js Github Issues](https://github.com/ReactiveX/rxjs/issues/2099)

[🔗 WebSocket 또한 ajax와 같은 Issue 존재](https://github.com/ReactiveX/rxjs/issues/3942)

https://gearheart.io/articles/auto-websocket-reconnection-with-rxjs-with-example/
