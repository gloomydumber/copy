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

## styled-components

### general

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

### as / attrs

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

### animation string interpolation

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

### pseudo selector

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

### ThemeProvider

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

## typescript on React

### general

for type protection

```bash
npx create-react-app appName --template typescript
yarn create react-app appName --template typescript
```

처음 create-react-app을 typescript template로 시작하는 방법

```bash
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
```

기존 create-react-app에 typescript를 새로 적용하는 방법

```bash
npm i --save-dev @types/styled-components
```

js로 작성된 module을 ts에서 사용할 때, ts지원 module을 설치 해줘야 함.

DefinitelyTyped 에서 오픈소스로 여러 모듈들의 typescript 지원 중

### interface

```tsx
// App.tsx
import Circle from "./Circle";

function App() {
  return (
    <div>
      <Circle bgColor="teal" /> // bgColor라는 props 이용 중
      <Circle bgColor="tomato" />
    </div>
  );
}

export default App;
```

```typescript
// Circle.tsx
import styled from "styled-components";

interface ContainerProps {
  bgColor: string;
}

const Container = styled.div<ContainerProps>`
  // styled-components 에서 Props Interface는 이런 방식으로.
  width: 200px;
  height: 200px;
  background-color: ${(props) => props.bgColor};
  border-radius: 100px;
`;

interface CircleProps {
  bgColor: string;
}

function Circle({ bgColor }: CircleProps) {
  // props에 type 부여를 위해 interface 정의 및 이용
  return <Container bgColor={bgColor} />;
}

export default Circle;
```

component의 props에 types을 주려면 interface를 이용

### default props / optional props

```typescript
// App.tsx
import Circle from "./Circle";

function App() {
  return (
    <div>
      <Circle borderColor="lime" bgColor="teal" />
      <Circle text="i'm here" bgColor="tomato" />
    </div>
  );
}

export default App;
```

```typescript
// Circle.tsx
import styled from "styled-components";

interface ContainerProps {
  bgColor: string;
  borderColor: string; // ContainerProps에서는 required props.
  text?: string;
}

const Container = styled.div<ContainerProps>`
  width: 200px;
  height: 200px;
  background-color: ${(props) => props.bgColor};
  border-radius: 100px;
  border: 1px solid ${(props) => props.borderColor};
`;

interface CircleProps {
  bgColor: string;
  borderColor?: string; // CircleProps에서는 Optional Props.
  text?: string;
}

function Circle({ bgColor, borderColor, text = "default text" }: CircleProps) {
  // = 을 통해 default props 이용.
  return (
    <Container
      bgColor={bgColor}
      borderColor={borderColor ?? bgColor} // borderColor가 undefined인 경우, bgColor를 전달
      text={text}
    >
      {text}
    </Container>
  );
}

export default Circle;
```

? 연산자를 통해, optional props 설정 가능. default props의 경우 typescript의 기능은 아니지만, props 전달시 = 연산자를 통해 설정 가능

?? 연산자를 통해, props가 undefined인 경우에 따로 props로 전달할 parameter 설정 가능

### React state with ts

```tsx
const [counter, setCounter] = useState(1); // 초기값을 1로 주었으므로, ts compiler에서 counter state는 number 값으로 예상
```

```tsx
const [counter, setCounter] = useState<number | string>(1); // number 또는 string type을 state로 가질 수 있음을 선언
```

default state를 설정해주면 해당 type만을 state로 추론해서 해당 type으로만 변경을 허용하고, 다른 type으로 setState를 시도하는 순간 ts compiler는 error notify

### ts on react example (form event)

```tsx
import React from "react";
import { useState } from "react";

function App() {
  const [value, setValue] = useState("");
  const onChange = (e: React.FormEvent<HTMLInputElement>) => {
    setValue(e.currentTarget.value); // React.js TS는 target이 아니라 currentTarget 사용
    // same as below
    // const {
    //   currentTarget: { value },
    // } = e;
    // setValue(value);
  };
  const onSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    console.log("hello", value);
  };
  return (
    <div>
      <form onSubmit={onSubmit}>
        <input
          value={value}
          onChange={onChange}
          type="text"
          placeholder="username"
        />
        <button>Log in</button>
      </form>
    </div>
  );
}

export default App;
```

React.FormEvent<HTMLFormElement> 와 같은 type setting이 필요함. 구글링 등으로 html event등이 ts에서는 어떤 type인지 알아내야 함.

React.js를 ts에서 사용할 경우 event.target이 아닌, event.currentTarget을 사용하기로 함 (event.target vs event.currentTarget)

### ts on react example (styled-components module)

react에 적용할 module에서의 ts 사용 예제로 styled-components에서 theme을 설정해본다.

```ts
//styled.d.ts
// import original module declarations
import "styled-components";

// and extend them!
declare module "styled-components" {
  export interface DefaultTheme {
    textColor: string;
    bgColor: string;
  }
}
```

[styled-components Official docs for ts](https://styled-components.com/docs/api#typescript){: target="\_blank"}

우선, 공식문서의 설명대로 styled.d.ts를 생성해서 설정 코드를 입력한다.

```typescript
// theme.ts
import { DefaultTheme } from "styled-components";

export const lightTheme: DefaultTheme = {
  textColor: "black",
  bgColor: "whitesmoke",
  btnColor: "teal",
};

export const darkTheme: DefaultTheme = {
  textColor: "lime",
  bgColor: "black",
  btnColor: "teal",
};
```

이후 theme.ts에서 DefaultTheme type의 theme 객체들을 설정해준다.

```tsx
// index.tsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { ThemeProvider } from "styled-components";
import { lightTheme, darkTheme } from "./theme";

ReactDOM.render(
  <React.StrictMode>
    <ThemeProvider theme={lightTheme}>
      <App />
    </ThemeProvider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

마찬가지로 index.tsx에서 ThemeProvider Component를 import하여 설정한다. theme객체 또한 import한다.

```tsx
// App.tsx
import React from "react";
import styled from "styled-components";

const Container = styled.div`
  background-color: ${(props) => props.theme.bgColor};
`;

const H1 = styled.h1`
  color: ${(props) => props.theme.textColor};
`;

function App() {
  return (
    <Container>
      <H1>protected</H1>
    </Container>
  );
}

export default App;
```

이후 props에서 theme props 객체로 접근하여 attribute들을 이용해 색상 접근 및 설정

c.f.) [React.js Official docs for events](https://reactjs.org/docs/events.html){: target="\_blank"} (React Synthetic Event)

## react-query

### general

the way of fetching data on React
