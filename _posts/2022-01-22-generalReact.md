---
layout: post
title: "React"
date: 2022-01-22 12:50:00
categories: WEB-FRONTEND
permalink: /archivers/React
nocomments: false
use_math: true
---

# React

studying react

hooks, error handling, etc...

editing...

## React 관련

https://github.com/skidding/cosmos component 개별 정리

https://sohyunsaurus.tistory.com/37 react 개발자라면 알아야하는 15가지 custom hooks

reddit.com/r/reactjs/redit react게

### React가 렌더링을 수행하는 시점

React 컴포넌트가 렌더링을 수행하는 시점은 다음과 같습니다.

1. Props가 변경되었을 때

2. State가 변경되었을 때

3. forceUpdate() 를 실행하였을 때

4. 부모 컴포넌트가 렌더링되었을 때

### react table 관련

https://www.daleseo.com/react-table/ react-table 관련

### react drang and drop 구현 관련

https://moong-bee.com/posts/react-drag-and-drop-list drag and drop 직접 구현

https://watermelonlike.tistory.com/176 react drag and drop 관련

https://www.biew.co.kr/entry/HTML5%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-%EB%93%9C%EB%9E%98%EA%B7%B8%EC%95%A4%EB%93%9C%EB%A1%AD react아님, html drag and drop 설명

### useEffect data fetching 관련

https://darrengwon.tistory.com/275 useEffect에서 데이터 fetching하기

useEffect에서 데이터 fetching시, race condition 문제 및 throttle, debounce

https://tecoble.techcourse.co.kr/post/2021-09-12-race-condition-handling/ race condition - 테코블

https://velog.io/@pius712/useEffect-race-condition-%EB%8B%A4%EB%A3%A8%EA%B8%B0 race condition

https://webclub.tistory.com/607 throttle, debounce

### useState

https://velog.io/@seongkyun/React%EC%9D%98-setState%EA%B0%80-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%B2%98%EB%A6%AC%EB%90%98%EB%8A%94-%EC%9D%B4%EC%9C%A0 useState가 비동기처럼 작동하는 이유

### useRef

https://velog.io/@juno7803/React-useRef-200-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0 useRef 200% 활용하기

### react에서 setInterval 사용하기 (useInterval Custom Hook)

https://iborymagic.tistory.com/96

https://bsnn.tistory.com/50

https://mingule.tistory.com/65

https://www.suzie.world/blog/making-setinterval-declarative-with-react-hooks

https://velog.io/@jakeseo_me/%EB%B2%88%EC%97%AD-%EB%A6%AC%EC%95%A1%ED%8A%B8-%ED%9B%85%EC%8A%A4-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%97%90%EC%84%9C-setInterval-%EC%82%AC%EC%9A%A9-%EC%8B%9C%EC%9D%98-%EB%AC%B8%EC%A0%9C%EC%A0%90

### useLayoutEffect

https://merrily-code.tistory.com/46 useLayoutEffect 에관하여

https://always-develop.tistory.com/73 useEffect와 useLayoutEffect 차이

### websocket 수신 데이터를 state로 이용할 때

https://stackoverflow.com/questions/66615601/react-websocket-is-there-a-way-to-throttle-to-limit-the-number-of-times-the-c

https://velog.io/@seongkyun/React-%EC%B5%9C%EC%A0%81%ED%99%94-buffer%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%98%EC%97%AC-%EC%83%81%ED%83%9C-%EA%B0%B1%EC%8B%A0-%EC%A4%84%EC%9D%B4%EA%B8%B0 buffer로 해결(업비트 클론코딩)

## References

[jbee.io](https://jbee.io/){: target="\_blank"}

https://react.vlpt.us/ react 공부 사이트 (벨로퍼트와함께하는모던리액트)

<!-- ![permasecond](/assets/posts/2020-02-21-cmdcolor/permasecond.png) -->

## from nomad

### styled-components

#### general

```javascript
function App() {
  return (
    <div style={{ display: "flex" }}>
      <div style={{ backgroundColor: "teal", width: 100, height: 100 }}></div>
      <div style={{ backgroundColor: "tomato", width: 100, height: 100 }}></div>
    </div>
  );
}

export default App;
```

위와 같이 작성해야할 CSS를, 아래와 같이 적용

```javascript
import styled from "styled-components";

const Father = styled.div`
  display: flex;
`;

const BoxOne = styled.div`
  background-color: teal;
  width: 100px;
  height: 100px;
`;

const BoxTwo = styled.div`
  background-color: tomato;
  width: 100px;
  height: 100px;
`;

const Text = styled.span`
  color: white;
`;

function App() {
  return (
    <Father>
      <BoxOne>
        <Text>Hello</Text>
      </BoxOne>
      <BoxTwo />
    </Father>
  );
}

export default App;
```

```javascript
import styled from "styled-components";

const Father = styled.div`
  display: flex;
`;

const Box = styled.div`
  background-color: ${(props) => props.bgColor};
  width: 100px;
  height: 100px;
`;

const Circle = styled(Box)`
  border-radius: 50px;
`;

function App() {
  return (
    <Father>
      <Box bgColor="teal" />
      <Circle bgColor="tomato" />
    </Father>
  );
}

export default App;
```

class명은 styled-components에서 unique한 이름으로 dynamically 생성. (duplication, overlap, misspelling 방지)

props, extend(상속과 유사) 사용 가능

CSS part와 Component 구현 part를 분리

#### as / attrs

```javascript
import styled from "styled-components";

const Father = styled.div`
  display: flex;
`;

const Input = styled.input.attrs({ required: true })`
  background-color: tomato;
`;

function App() {
  return (
    <Father as="header">
      // 원래, father는 div로 작성되었지만, as로 header tag로 render
      <Input /> // attrs를 통해 require: true 속성을 한 번에 부여
      <Input />
      <Input />
      <Input />
      <Input />
    </Father>
  );
}
export default App;
```

as를 사용함으로써, CSS 속성은 유지하되 다른 tag로 작성되도록 할 수 있음

attrs를 사용함으로써, tag에 하나하나 속성 부여할 필요 없이, 한 번에 부여

#### animation string interpolation

```javascript
import styled, { keyframes } from "styled-components";

const Wrapper = styled.div`
  display: flex;
`;

const rotationAnimation = keyframes`
  0% {
    transform: rotate(0deg);
    border-radius: 0px;
  }
  50% {
    border-radius: 100px;
  }
  100%{
    transform: rotate(360deg);
    border-radius: 0px;
  }
`;

const Box = styled.div`
  height: 200px;
  width: 200px;
  background-color: tomato;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: ${rotationAnimation} 1s linear infinite; // animation을 string interpolation으로 처리
  span {
    font-size: 150px;
    &:hover {
      // & 로 간편하게 액션 처리 가능
      font-size: 50px;
    }
  }
`;

function App() {
  return (
    <Wrapper>
      <Box>
        <span>🧐</span>
      </Box>
    </Wrapper>
  );
}

export default App;
```

animation을 따로 정의하여, string interpolation으로 처리 가능

HTML 요소안의 다른 요소에도 CSS 적용 가능, 또 내부 scope에 '&'로 그러한 요소에 액션까지 간편하게 추가 가능 (pseudo selector)

#### pseudo selector

```javascript
import styled, { keyframes } from "styled-components";

const Wrapper = styled.div`
  display: flex;
`;

const rotationAnimation = keyframes`
  0% {
    transform: rotate(0deg);
    border-radius: 0px;
  }
  50% {
    border-radius: 100px;
  }
  100%{
    transform: rotate(360deg);
    border-radius: 0px;
  }
`;

const Emoji = styled.span`
  font-size: 150px;
`;

const Box = styled.div`
  height: 200px;
  width: 200px;
  background-color: tomato;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: ${rotationAnimation} 1s linear infinite;
  ${Emoji} {
    &:hover {
      // Component로 분리해도 다른 Component 내부에서 적용 가능
      font-size: 50px;
    }
  }
`;

function App() {
  return (
    <Wrapper>
      <Box>
        <Emoji as="p">🧐</Emoji>
        {/* as로 다른 HTML tag로 작용하더라도 CSS 효과, Animation은 그대로 적용 */}
      </Box>
      <Emoji as="p">🧐</Emoji>
      {/* Box밖의 Emoji는 CSS Animation 효과가 적용되지 않음. */}
    </Wrapper>
  );
}

export default App;
```

다른 Component 내부에 있는 Component의 CSS도 정의할 수 있음 (pseudo selector)

#### ThemeProvider

```javascript
// index.js

import React from "react";
import ReactDOM from "react-dom/client";
import { ThemeProvider } from "styled-components"; // ThemeProvider를 import 해야 함.
import App from "./App";

// darkTheme & lightTheme 객체를 선언, 속성값이 같아야함.

const darkTheme = {
  textColor: "whitesmoke",
  backgroundColor: "#111",
};

const lightTheme = {
  textColor: "#111",
  backgroundColor: "whitesmoke",
};

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <ThemeProvider theme={darkTheme}>
      {" "}
      // theme props
      <App />
    </ThemeProvider>
  </React.StrictMode>
);
```

```javascript
import styled, { keyframes } from "styled-components";

const Title = styled.h1`
  color: ${(props) => props.theme.textColor}; // theme props 이용
`;

const Wrapper = styled.div`
  display: flex;
  height: 100vh;
  width: 100vw;
  justify-content: center;
  align-items: center;
  background-color: ${(props) =>
    props.theme.backgroundColor}; // theme props 이용
`;

function App() {
  return (
    <Wrapper>
      <Title>Hello</Title>
    </Wrapper>
  );
}

export default App;
```

styled-components에서 ThemeProvider를 제공 함. theme 객체들을 생성하고 props로 넘겨주면, 편리한 theme 기능 이용 가능
