---
layout: post
title: "reactiveX - retry vs repeat"
date: 2022-08-02T00:00:00Z
categories: ReactiveX
permalink: /archivers/rxjsRetryRepeat
nocomments: false
use_math: true
---

# rxjs AJAX & WebSocket on Node.js

| ![reactivex](/assets/posts/2022-08-02-retryAndrepeat/reactivex_logo.png) |
| :----------------------------------------------------------------------: |
|                 <b>rxjs AJAX & WebSocket on Node.js</b>                  |

## rxjs is for browser library

우선, `rxjs`는 본래 _browser_ 환경을 위해 구현된 라이브러리 이므로, `Node.js` 환경에서 `AJAX` 또는 `WebSocket`을 이용하기 위해서는 다음과 같은 설정이 필요하다.

### AJAX

`AJAX`를 이용하기 위해서는, `xhr2` 모듈을 `rxjs` 라이브러리에 정의 된 `XMLHttpRequest` 객체를 replace해야한다.

```javascript
global.XMLHttpRequest = require("xhr2"); // for Server Side Ajax
const { ajax } = require("rxjs/ajax");

const obs$ = ajax("https://api.github.com/users?per_page=5");
obs$.subscribe((result) => console.log(result.response));
```

### WebSocket

`WebSocket`의 경우도 마찬가지로, `ws` 모듈을 `rxjs` 라이브러리에 정의 된 `WebSocket` 객체를 replace 해주고 난 다음 사용한다.

```javascript
global.WebSocket = require("ws");
const { webSocket } = require("rxjs/webSocket");
const webSocketSubject = webSocket("ws://localhost:8001");
webSocketSubject.subscribe(console.log);
```

## TL;DR

`rxjs`에서 `AJAX`와 `WebSocket`은 _browser_ 환경을 위해 구현되어 있기 때문에, `Node.js`에서 사용하려면 `AJAX`와 `WebSocekt`을 각각 `xhr2`, `ws` 모듈 등으로 `override`해서 사용해야 한다.

## References

[🔗 rxjs/ajax on Node.js Github Issues](https://github.com/ReactiveX/rxjs/issues/2099)

[🔗 rxjs/WebSocket on Node.js Github Issues](https://github.com/ReactiveX/rxjs/issues/3942)
