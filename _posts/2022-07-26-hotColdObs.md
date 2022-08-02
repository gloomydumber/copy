---
layout: post
title: "Hot vs Cold Observables"
date: 2022-07-26T00:00:00Z
categories: ReactiveX
permalink: /archivers/hotColdObs
nocomments: false
use_math: true
---

# Hot vs Cold Observables

| ![reactivex](/assets/posts/2022-07-26-hotColdObs/reactivex_logo.png) |
| :------------------------------------------------------------------: |
|                <b>🥶 Observable VS. 🔥 Observable</b>                |

이 내용은 동일한 제목의 영어 원문 [_Medium_ 포스트](https://benlesh.medium.com/hot-vs-cold-observables-f8094ed53339){: target="\_blank"}를 번역한 것임

## COLD is when your observable creates the producer

`Cold` 방식은 `Observable`이 `producer`를 생성할 때를 일컫음

```javascript
// COLD 🥶
var cold = new Observable((observer) => {
  var producer = new Producer();
  // have observer listen to producer here
});
```

## HOT is when your observable closes over the producer

`Hot` 방식은 `Observable` 밖에서 `producer`가 존재할 때를 일컫음

```javascript
// HOT 🔥
var producer = new Producer();
var hot = new Observable((observer) => {
  // have observer listen to producer here
});
```

## Getting deeper into what’s going on…

`Hot`과 `Cold`? 이것이 무슨 의미인지 자세히 알아보자...

[Observable 만들면서 Observable 배우기](https://medium.com/@benlesh/learning-observable-by-building-observable-d5da57405d87){: target="\_blank"} 라는 포스트에서 `Observable`이 단순한 `function`임을 설명했다. 그 포스트에서의 목표는 `Observable` 자체에 대한 설명이었는데, 대부분의 사람들이 `Observable`에 대해 혼동하고 있는 `Hot`과 `Cold`의 개념에 대해서는 설명하지 않았다.

## Observables are just functions!

`Observable`은 그냥 함수일 뿐이다.

`Observable`은 `producer`와 `observer`를 서로 엮어주는 함수일 뿐이다. 그게 다다. `Observable` 내부에서 필수적으로 `producer`를 생성할 필요는 없고, 단지 `producer`를 수신하는 `observer`를 설정한다. 그리고 나서 일반적으로는, `listenr`를 제거하기 위한 분해 메커니즘을 반환한다. (역자 주 : 아마 `complete`를 의미하는 것 같다) `subscription` 하는 행위는 `Observable`을 함수처럼 호출하고, 구독 값들을 `Observer`에게 전달하는 것이다.

## What's a "Producer"?

`Producer`가 무엇인가?

`Producer`는 `Observable`에 발행될 값들의 원천(_source_)이다. `Websocket`이 될 수도 있고, `DOM events`가 될 수도 있으며, `iterator`가 될 수도 있고, 단순히 `array`를 순회하는 것일 수도 있다. 기본적으로, `Observer.next(value)`와 같은 방법으로 전달되는 모든 형태의 값을 생성하는 것을 뜻한다.

## Cold Observables: Producers created _inside_

`Cold Observable` : `Producer`가 내부적으로 생성됨

`Cold Observable`은 구독 중에 `producer`가 생성되고 활성화 되는 `Observable`을 뜻한다. 즉, 앞서 언급했듯이 `Observable`을 `function`에 비유하자면 *함수 호출*을 통해서 `producer`가 생성되고 활성화되는 것을 의미한다.

- ① creates the producer (producer를 생성)
- ② activates the producer (producer를 활성화)
- ③ starts listening to the producer (producer 수신을 시작)
- ④ unicast (단일 발신자 <-> 단일 수신자)

`Observable`을 구독하는 순간에서야 `Observable` 선언 *내부*에서 `WebSocket`이 생성되고 수신되므로, 아래의 예제는 `cold`로 작성된 것이다.

```javascript
const source = new Observable((observer) => {
  const socket = new WebSocket("ws://someurl");
  socket.addEventListener("message", (e) => observer.next(e));
  return () => socket.close();
});
```

어떤 것이든 위 예제 코드의 `source`라는 변수의 `Observable`을 구독하면, 그 구독에 대한 독립적인 `WebSocket Instance`를 가지게 되는 것이고, 구독을 해지(_unsubscribe_)하면, 소켓 연결을 `close()`할 것이다. 이게 바로 `source`가 `unicast` 방식으로 구현되어 있다는 것을 의미한다. `producer`가 오직 한 `observer`에게만 값을 발행해줄 수 있기 때문이다. ([Cold Observable Example Codes JSBin](http://jsbin.com/wabuguy/1/edit?js,output){: target="\_blank"})

## Hot Observables: Producers created _outside_

`Hot Observable` : `Producer`가 외부에서 생성됨

구독의 바깥 부분에서 생성되고 활성화되는 `producer`를 이용하는 `Observable`을 `Hot Observable`이라 칭한다.

- ① shares a reference to a producer (`producer`의 값을 `share`하는 형태)
- ② starts listening to the producer (`producer`의 발행을 수신하기 시작)
- ③ multicast (복수의 수신자)

다음 예제 코드와 같이, `WebSocket`의 생성을 `Observable`의 정의 외부에 한다면 `hot Observable`이 된다.

```javascript
const socket = new WebSocket("ws://someurl");
const source = new Observable((observer) => {
  socket.addEventListener("message", (e) => observer.next(e));
});
```

이제 `source` 변수의 `producer`를 구독하는 모든 것들은 같은 `WebSocket Instance`를 공유하는 것이다. 모든 구독자들에게 `unicast`되고 있는 것이다. 그런데, 이 방법의 문제점은 더 이상 `Observable`이 `WebSocket`에 관한 연산을 할 수 없다. 그러니까, `Observable`로는 `error`나 `complete`나 `unsubscribe`를 할 수 없다. 더 이상 `Observable`로는 소켓을 닫을 수 없다. 우리가 진짜 원하는 것은 우리들의 `cold Observable`을 `hot`의 형태로 만드는 것이다. (역자 주 : `producer`를 `Observable` 내부에 선언하되, `unicast` 방식이 되기를 원한다는 의미로 보임) ([Hot Observable Example Codes JSBin](http://jsbin.com/godawic/edit?js,output){: target="\_blank"})

## Why Make A "Hot" Observable?

왜 `Hot Observable`을 생성하는가?

`Cold Observable`의 에제에서 보았듯이, 모든 때에 모든 `Observable`을 `Cold`로 구현하는 것은 문제가 있어보일 것이다. 한 가지 예를 들어, `WebSocket` 연결을 통해 수신 되는 값을 홀수와 짝수로 구분해야한다고 생각해보자. 이 경우 2 개의 `Observable`을 생성하고 2개의 구독을 해야하는 문제가 있다. 한 번의 구독을 통해 분류하는 것이 컴퓨팅 자원을 효율적으로 활용하는 것임은 자명하다.

```javascript
source.filter((x) => x % 2 === 0).subscribe((x) => console.log("even", x));
source.filter((x) => x % 2 === 1).subscribe((x) => console.log("odd", x));
// 두 번의 구독이 발생함
```

## Rx Subjects

`Cold Observable`을 `Hot`으로 만들기 전에, (`multicast`로 동작하도록 만들기 전에,) `rx`의 `Subject` 타입에 대해 알아보자. `Subject`는 다음과 같은 속성이 있다.

- ① `Observable`이다. `Observable`과 형태가 유사하고, 적용할 수 있는 `Operator` 또한 완전 동일하다.
- ② `Observer`이다. `Observer`를 `duck-typing`한다. 즉, `Observer`처럼 동작한다. `Observable`로서 구독되면, `next()`로 전달하는 값을 발행 및 수신을 동시에 한다.
- ③ `multicast`한다. `subscribe()`를 통해 전달된 모든 `Observer`가 내부 `Observer` 리스트에 등록된다.
- ④ 끝나면 끝난거다. `Subjcet`는 `unsubscribe`, `complete`, `error` 이후에는 재사용될 수 없다.
- ⑤ 바로 앞의 2번 에서 언급했듯이, 그 자체로 값을 발행하면서도 수신도 한다.

일반적으로, `Subject`는 4개의 Observer-Pattern 안에서 `addObserver` 메소드와 함께 객체로 정의 되어있다. 여기서는 `addObserver` 메소드가 `subscribe`로 구현되어있다. ([Rx Subject Behavior JSBin](http://jsbin.com/godawic/edit?js,output){: target="\_blank"})

## Making A Cold Observable Hot

`Cold Observable`을 `Hot`으로 만들기

`Subject`에 대한 이해를 바탕으로, `functional Programming`의 방법을 약간 사용해서 `Cold Observable`을 `Hot`으로 동작하도록 만들 수 있다.

```javascript
function makeHot(cold) {
  const subject = new Subject();
  cold.subscribe(subject);
  return new Observable((observer) => subject.subscribe(observer));
}
```

위 예제의 `makeHot` 함수를 통해서, 이 함수에 어떤 `Cold Observable` 이든간에 인자로 전달해서 `Subject`를 생성하여 `Hot`으로 바꿔 줄 수 있다. ([Cold -> Hot Example Codes JSBin](http://jsbin.com/ketodu/1/edit?js,output){: target="\_blank"})

그럼에도 아직 문제점은 있는데, 구독이 `source`를 트랙킹하고 있지 않다. 어떻게 해당 `Observable`을 우리가 원하는 시점에 해제 및 종료 (_tear down_) 할 수 있을까? 이를 위해서는 다음과 같이 `reference counting`을 이용해 구현한다.

```javascript
function makeHotRefCounted(cold) {
  const subject = new Subject();
  const mainSub = cold.subscribe(subject);
  let refs = 0;
  return new Observable((observer) => {
    refs++;
    let sub = subject.subscribe(observer);
    return () => {
      refs--;
      if (refs === 0) mainSub.unsubscribe();
      sub.unsubscribe();
    };
  });
}
```

이제 `Observable`을 `hot`으로 구현하고, 원할 때에 `Observable`을 종료시킬 수 있다. *refs*라는 변수가 -0 이되면 `Cold Observable`의 `source`가 제대로 `unsubscribe`될 것이다. ([Cold -> Hot Example Codes with Reference Counting JSBin](http://jsbin.com/lubata/1/edit?js,output){: target="\_blank"})

## In RxJS, Use `publish()` or `share()`

`RxJS`에서는 `publish()`나 `share()`를 이용하면 된다.

앞선 구현처럼 `makeHot` 함수는 아마도 사용되지 않을 것이다. 같은 동작이 `publish()`나 `share()`라는 `operator`로 구현되어 있기 때문이다. `Cold Observable`을 `hot`으로 동작하게 하는 방법은 아주 많다. 그래서 `RxJS`는 그러한 동작을 효율적이고 간결하게 동작하게끔 `operator`로 구현해 놓은 것이다.

`RxJS 5`에서 `share()` 연산자는 `Observable`을 `hot`으로 만들어주며, `refCounted Observable`이 실패시에는 재시도를, 성공시에는 반복하도록 구현되어있다. 이는 `Subject`는 에러가 발생하거나 완료되거나 구독 해지 되는 경우에는 재사용할 수 없기 때문이다. `share()` 연산자는 죽은 `Subject`를 재활용하기 위해서 재구독을 가능하게끔 구현되었다.

[`share()` Example Codes JSBin](http://jsbin.com/mexuma/1/edit?js,output){: target="\_blank"}

## The "Warm" Observable

따뜻한 `Observable`

하나의 구독으로 처리 할 수 있는 것을 두 개의 `Subject`를 만들어야 하는 일은 나쁜 미신이다.

## "Hot" and "Cold" Are All About The Producer

"Hot"과 "Cold"가 `producer`의 전부이다.

`Observable` 안에서 `shared reference`를 `close`한다면, `hot` 방식이다. `Observable` 내부에서 새로운 `producer`를 생성한다면 `cold` 방식이다. 만약에 둘다한다면... 뭘 하는거지? 아마도 `Warm` 방식인 것 같다.

## TL;DR

직접 `producer`를 계속해서 생성할 것이 아니라면, `Observable`을 `HOT` 방식으로 사용하게 될 것이다.

## References

[Hot vs Cold Observables 원문](https://benlesh.medium.com/hot-vs-cold-observables-f8094ed53339){: target="\_blank"}

[Observable 만들면서 Observable 배우기](https://medium.com/@benlesh/learning-observable-by-building-observable-d5da57405d87){: target="\_blank"}

[Cold Observable Example Codes JSBin](http://jsbin.com/wabuguy/1/edit?js,output){: target="\_blank"}

[Hot Observable Example Codes JSBin](http://jsbin.com/godawic/edit?js,output){: target="\_blank"}

[Rx Subject Behavior JSBin](http://jsbin.com/godawic/edit?js,output){: target="\_blank"}

[Cold -> Hot Example Codes JSBin](http://jsbin.com/ketodu/1/edit?js,output){: target="\_blank"}

[Cold -> Hot Example Codes with Reference Counting JSBin](http://jsbin.com/lubata/1/edit?js,output){: target="\_blank"}

[`share()` Example Codes JSBin](http://jsbin.com/mexuma/1/edit?js,output){: target="\_blank"}
