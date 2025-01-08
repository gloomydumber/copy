---
layout: post
title: "HTTP Persistent Connection on Node.js / Rust"
date: 2025-01-07 10:40 +0900
categories: WEB-BACKEND
permalink: /archivers/prstcxn
nocomments: false
use_math: true
---

# HTTP Persistent Connection on Node.js / Rust

|    ![keepAlive](/assets/posts/2025-01-07-persistentConnection/keepAlive.gif)     |
| :-----------------------------------------------------------------: |
| <b>Let's Keep hope Alive</b> |

## HTTP Keep-Alive / Persistent Connection

HTTP 연결은 TCP에 기반해 있고, TCP 커넥션은 연결 성립 이후 자체적으로 '튜닝' 되는데, 연결 초기에는 커넥션의 최대 속도를 제한하고 데이터가 성공적으로 전송됨에 따라서 속도 제한을 높여나간다. 이러한 TCP의 특징을 느린 시작(Slow Start)라 하는데, 이는 Congestion Control 알고리즘의 일부로 Congestion Control 알고리즘은 인터넷의 급작스러운 부하와 혼잡을 방지하는 역할을 한다.

이러한 혼잡 제어 기능 떄문에, 새로운 커넥션은 이미 어느 정도 데이터를 주고받은 '튜닝' 된 커넥션보다 느리다. '튜닝'된 커넥션은 막 성립된 새로운 커넥션 보다 더 빠르기 때문에, HTTP에는 이미 존재하는 커넥션을 재사용하는 기능이 있다.

HTTP 통신은 TCP 커넥션 위에서 이루어지는 것이고, HTTP 통신 자체는 비연결성(connectionless) 통신이지만, TCP 통신은 연결 지향(Connection-oriented) 통신이기 때문에 TCP라는 비교적 지속적인 연결 위에서 각각의 HTTP 통신이 단절적(비연결성)으로 이루어지고 있다고 이해할 수 있다.

따라서, HTTP 통신이 비연결성을 지니고 있으면서도, TCP의 연결 지향적인 특징을 활용하면, 좀 더 효율적인 통신을 할 수 있다.

가령, API 요청을 HTTP 프로토콜을 통해 진행 할 때, 같은 서버에 요청을 3번 해야하는 경우, 3번 각각 동안 각기 다른 TCP 연결을 성립시키는 방법도 있지만, 단 한 번만 TCP 연결을 성립시키고, 해당 연결 상에서 3번의 모든 HTTP 연결을 처리할 수 있다는 것이다.

전자와 같이 3번 동안 각각 각기 다른 TCP 연결을 성립하고자하면, 각 연결마다 '튜닝'이 진행되어야 하기 때문에, 후자와 같이 이미 성립 된 최초의 TCP 연결을 활용하는 것보다 비효율적이고 느릴 것이다.

이렇게, 이미 성립 된 최초의 TCP 연결을 활용하는 것을 Keep-Alive Connection 및 Persistent Connection이라 부른다.

다만, 이러한 Persistent Connection은 클라이언트 및 서버의 사양이나 로직에 따라 제멋대로 연결이 해제될 수 있기 때문에 이를 잘 고려하여 설계해야한다.

이러한 기능을 HTTP/1.0 버전에서는 헤더에 `keep-alive` 표기를 하는 방식으로 동작하였고, HTTP/1.1 버전에서부터는 헤더에 별도의 설정이 없어도 기본적으로 `Persistent Connection` 을 지원하게 되었다.

이러한 HTTP 통신의 Persistence Connection을, Node.js 및 Rust의 대표적인 모듈 / 라이브러리 / 크레이트 에서는 어떻게 다루고 있는지 살펴보았다.

## Node.js (`http` & `https`)

Node.js에서 `npm` 등을 통해 별도의 설치 없이 기본으로 제공하는 `http` 및 `https` 모듈은 HTTP 요청 및 응답을 다루도록 하는 기능을 제공한다.

아래는 `http` 및 `https` 모듈을 이용해 `GET` 요청을 하는 예제이다. (본 포스트의 예제 코드는 단순 설명을 위한 PoC의 형태이므로 실동작에는 오류가 있을 수 있다)

```js
const https = require('https');

// Define the URL
const url = 'https://jsonplaceholder.typicode.com/posts/1';

// Make the GET request
https.get(url, (res) => {
  let data = '';

  // Handle incoming data chunks
  res.on('data', (chunk) => {
    data += chunk;
  });

  // Handle the end of the response
  res.on('end', () => {
    console.log('Response:', JSON.parse(data)); // Parse and display the response
  });
}).on('error', (err) => {
  console.error('Error:', err.message); // Handle errors
});
```

하지만 위와 같이 작성하는 경우에는, Persistence Connection을 성립시키지 못한다.

물론, 예제 코드에서는 단 한번의 HTTP 요청만이 이루어지고 있으니, 애초에 Persistence Connection의 방식으로 요청할 필요는 없지만 곧바로 다시 `https://jsonplaceholder.typicode.com` 이라는 동일한 서버에 또 다른 요청을 보내야 한다고 가정해보자.

```js
const https = require('https');

// Define the agent
const agent = new https.Agent({
  keepAlive: true, // Keep the connection alive for reuse
});

// Define the request options
const options = {
  hostname: 'jsonplaceholder.typicode.com',
  path: '/posts/1',
  method: 'GET',
  agent, // Use the custom agent
};

// Make the HTTPS request
const req = https.request(options, (res) => {
  let data = '';

  // Handle incoming data chunks
  res.on('data', (chunk) => {
    data += chunk;
  });

  // Handle the end of the response
  res.on('end', () => {
    console.log('Response:', JSON.parse(data)); // Parse and display the response
  });
});

// Handle errors
req.on('error', (err) => {
  console.error('Error:', err.message);
});

// End the request
req.end();

// 미리 생성된 TCP 연결 활용!
// `options`의 `agent` 변수가 다시 재활용 된다.
const secondReqWithPersistentConnection = https.request(options, (res) => { ... }) 
```

이번에는 `new https.Agent()` 를 통해, `keepAlive` 옵션을 `true`라는 값으로 명시적으로(explictly) 작성해주었다.

이렇게 명시적으로 작성해주어야 Persistence Connection이 성립되고, 미리 불변(`const`)으로 선언해둔 `agent` 변수를 활용하여 다시 요청을 하면 기존 성립된 TCP 연결을 활용하는 HTTP 통신을 할 수 있다.

앞서 언급하였듯이, HTTP 프로토콜 차원에서는 버전 HTTP/1.1 부터는 별도의 설정없이 Persistence Connection을 제공해준다고 하였다.

이는 프로토콜 레벨에서 그러하다는 의미이지, Node.js의 `http` 및 `https` 모듈에서 단지 아무 설정없이 `http.get()` 및 `https.get()` 을 여러 번 호출한다고해서 자동적으로 Persistente Connection을 유지시켜준다는 의미가 아니다.

아래는 Node.js의 `http`에서 제공하는 `Agent` 구현체의 일부이다. ([_http_agent.js - nodejs Official Github Repo.](https://github.com/nodejs/node/blob/8ed50bcbe522c6ceddf8e760b7fbe91b6adae81f/lib/_http_agent.js#L101))

```js
this.keepAlive = this.options.keepAlive || false;
```

사용자가 설정한 값을 갖되, 설정이 없을시에는 기본적으로는 `false` 값이 적용되어 있다.

한편, [Node.js 공식 문서의 new Agents() 항목](https://nodejs.org/api/http.html#new-agentoptions)을 보면, 옵션에서 나타내는 `KeepAlive` 필드와 HTTP 헤더에서의 `keep-alive` 값을 혼동하지말라는 내용이 있다.

> `keepAlive` <boolean> Keep sockets around even when there are no outstanding requests, so they can be used for future requests without having to reestablish a TCP connection. Not to be confused with the `keep-alive` value of the Connection header. `The Connection: keep-alive` header is always sent when using an agent except when the Connection header is explicitly specified or when the keepAlive and maxSockets options are respectively set to `false` and `Infinity`, in which case `Connection: close` will be used. Default: `false`.

헤더에 `keep-alive`가 표기되어 있다고 본인이 Persistence Connection을 이용하고 있다고 착각할 수 있다. 소위 *멍청한 프록시* 등 여러 요인이 있지만 헤더 자체에 `keep-alive` 가 표기되어있다고 해서, 실제로 HTTP 통신들이 동일한 단일의 TCP 연결을 활용하여 Persistence Connection이 이루어지고 있는지는 별도의 문제다.

가령, 두 번째 예제 코드에서 각각의 서버 호출에서 각기 다른 `agent` 를 `keepAlive` 옵션을 `true`로 설정하고 보내면, 두 호출모두 헤더 자체에는 `keep-alive`라고 표기되어 있겠지만, 그 둘은 별개의 TCP 연결을 통해 이루어진 HTTP 요청이다.

## Node.js (`request` 및 `axios`)

기본적으로 `request` 및 `axios` 등 비동기 호출 라이브러리들은 앞서언급한 Node.js의 `http` 및 `https`에 기반한다.

즉, `request`든 `axios` 든 호출 그 자체는 내부적으로 `http` 및 `https`의 설정을 통해 이루어진다.

`request` 라이브러리의 경우, *depreacted* 된 옛날 모듈이기 때문에 어떻게 구현되었나 살펴보기에 시의성이 적절하지 않지만, 굳이 따져보자면 Keep Alive라는 용어가 아닌, `forever` 라는 이름의 옵션 필드를 통해 Persistence Connection을 설정한다.

`axios` 라이브러리의 경우, 아래와 같이 `http` 및 `https` 모듈의 `Agent`를 `KeepAlive` 옵션에 `true`를 주어 Axios Instance 생성하거나, 요청 옵션에 `Agent`를 주입하는 등 두 가지 방법 등으로 구현한다.

```js
// 1. on the instance
const instance = axios.create({
  baseURL: 'jsonplaceholder.typicode.com',
//   httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),
});

instance.get('/')
    .then(res => {
        // ...
    })
    .catch(error => {
        // ...
    });

// 미리 생성된 TCP 연결 활용!
    instance.get('/')
    .then(res => {
        // ...
    })
    .catch(error => {
        // ...
    });


// 2. on the request
const httpsAgent = new https.Agent({ keepAlive: true });
axios.get('https://jsonplaceholder.typicode.com', { httpsAgent })
  .then(function (response) {
    // handle success
    console.log(response);
  })
  .catch(function (error) {
    // handle error
    console.log(error);
  })

// 미리 생성된 TCP 연결 활용!
axios.get('https://jsonplaceholder.typicode.com', { httpsAgent })
  .then(function (response) {
    // handle success
    console.log(response);
  })
  .catch(function (error) {
    // handle error
    console.log(error);
  })
```

또, 아래와 같이 `timeout` 및 `freeSocketTimeout` 등 Persistence Connection 관련 여러 설정을 할 수 있다.

```js
const agentWithOptions = new Agent({
  timeout: 60000,
  freeSocketTimeout: 30000,
});
```

이렇듯 각 라이브러리마다 Keep Alive, Persistence Connection, forever 등 각기 다른 용어를 쓰고 있고, 설정 방법이 큰 맥락은 동일하나 세부적으로는 다를 수 있고, Persistence Connection의 특성 상 헤더의 `keep-Alive` 값을 통해 확인하는 것이 아니라 직접 성립되었는지 확인도 필요하므로 실제로 Persistence Connection이 성립되었는지 꼼꼼한 구현이 요구된다.

## Rust (`tokio` 및 `reqwest`, `hyper` 등)

먼저 각 라이브에 대해 아주 간단하게 서술하자면,

`tokio`는 Rust 비동기 프로그래밍에서 사용되는 `runtime`이고, `reqwest`는 `tokio`를 기반으로한 HTTP Client Library이다.

한편, `hyper`는 좀 더 lower level에서의 HTTP 구현을 담당하는데, 버전 1.0 이전에는 `tokio`를 비동기 런타임으로 사용하였지만, 이제는 runtime-agnostic하게 변경되어 어떤 런타임이든 사용할 수 있다.

이 포스트에서는 `reqwest`의 Persistence Connection 예제 코드를 다룬다.

아래는 `reqwest`의 공식 문서에서 제공하는 [Making a GET Request](https://docs.rs/reqwest/latest/reqwest/#making-a-get-request) 예제 코드이다.

```rs
let body = reqwest::get("https://www.rust-lang.org")
    .await?
    .text()
    .await?;

println!("body = {body:?}");
```

바로 아래에 아래와 같이 keep-alive Cconnection pooling을 이용하려면, `Client`를 만들고 사용하라고 서술되어 있다.

> **NOTE:** If you plan to perform multiple requests, it is best to create a `Client` and reuse it, taking advantage of keep-alive connection pooling.

이를 반영하면,

```rust
let client = reqwest::Client::new();

let body = client.get("https://www.rust-lang.org")
    .await?
    .text()
    .await?;

println!("body = {body:?}");

let second_body_with_persis_cxn = client.get("https://www.rust-lang.org")
    .await?
    .text()
    .await?;

println!("persist connection body = {body:?}");
```

이와 같이 `Client`를 이용해 Persistence Connection을 이용할 수 있다.

또, 아래와 같이 `Clinet::builder()`를 이용해 Persistence Connection 관련 여러 설정이 가능하다.

```rust
let client = reqwest::Client::builder()
    .pool_max_idle_per_host(5) // Limit idle connections per host
    .pool_idle_timeout(Duration::from_secs(30)) // Set idle timeout
    .build()?;
```

[reqwest 공식 문서의 `pool_idle_timeout`](https://docs.rs/reqwest/latest/reqwest/struct.ClientBuilder.html#method.pool_idle_timeout)에 따르면 디폴트값으로 90초가 설정되어있고, `None`을 전달하면 tiemout을 두지 않는 것으로 한다.

이는 앞서언급하였듯이 서버 및 클라이언트가 이를 허용해주냐에 따라 다를 수 있으므로 주의해서 이용한다.

## References

[🔗 TCP Congestion Control - Wikipedia](https://en.wikipedia.org/wiki/TCP_congestion_control)

[🔗 HTTP Persistent Connection - Wikipedia](https://en.wikipedia.org/wiki/HTTP_persistent_connection)

[🔗 reqwest crate Official docs](https://docs.rs/reqwest/latest/reqwest/)