---
layout: post
title: "TypeScript"
date: 2022-07-24 09:00:00
categories: WEB-BACKEND
permalink: /archivers/typescript
nocomments: false
use_math: true
---

# TypeScript

Studying TypeScript and Memo

## commands

### initialize

```bash
tsc init
```

`tsconfig.json` 생성

### watch mode

```bash
tsc ${filename} -w
```

원본 _.ts_ 파일이 수정될 때 마다, _.js_ 파일로 컴파일

## tsconfig.json

`tsconfig.json`의 *property*에 관해 정리

### outDir

`compilerOptions`, 컴파일 완료 된 `.js` 파일을 출력할 경로

### rootDir

`compilerOptions`, 컴파일을 실행할 위치, 지정한 위치 상위에 `.ts` 파일이 있으면 에러 반환

### incremental

`compilerOptions`, 이전 컴파일과 비교해서 수정된 사항에 안해서만 컴파일 실행

### target

`compilerOptions`, 어떤 버전으로 컴파일 할 것인지 설정. 하위호환을 위해 상황에 관계 없이 낮은 버전을 사용하는 것은 지양. `ES5` 혹은 `ES6`를 많이 씀

### module

`compilerOptions`, `Node.js` 프로젝트의 경우 `CommonJS` (import가 아닌 require 이용), 브라우저 환경일 경우 `ES` 표준에 맞게 설정

### lib

`compilerOptions`, 세부적으로 어떤 `library`를 이용할 것인지 설정. 보통은 따로 설정하지 않고 `target` 설정시 사용되는 기본 `lib`를 사용

### allowJs

`compilerOptions`, 프로젝트 안에서 `.js`를 같이 사용할 것인지 여부 설정

### checkJs

`compilerOptions`, `.js` 파일에서 에러가 있다면 경고 표시

### jsx

`compilerOptions`, `react`에서 사용되는 `.jsx` 파일에 관한 설정

### declaration

`compilerOptions`, `type` 정의 관련. 작성한 코드가 라이브러리로서 사용되는 경우에 주로 사용. 일반 프로젝트에서는 사용하지 않음

### sourceMap

`compilerOptions`, 디버깅을 위해서 사용

### outFile

`compilerOptions`, 작성한 여러 `.ts`를 하나의 `.js`로 컴파일할 때 사용

### composite

`compilerOptions`, `incremental`과 같이 사용. 빌드 정보를 저장하여 이후에 있을 빌드를 빠르게 할 수 있도록 함

### tsBuildInfoFIle

`compilerOptions`, `incremental`의 설정이 `true`일 경우 그에 관련된 정보를 저장하는 파일을 출력하도록 설정

### removeComments

`compilerOptions`, 주석을 제거할것인지의 설정

### noEmit

`compilerOptions`, 컴파일 에러 체크만 하고, `.js` 파일을 반환하지는 않음. 컴파일 에러가 있는지 없는지 확인만 하고 싶을 때 사용

### isolatedModules

`compilerOptions`, 각각의 파일을 다른 `module`로 변환해서 컴파일 실행하도록 하는 설정

### exclude

컴파일을 제외할 파일

### include

컴파일을 할 파일. 이외의 것들은 컴파일 되지 않음

## References

[tsconfig.json Property 알아보기](https://it-eldorado.tistory.com/128){: target="\_blank"}

[tsconfig.json Property 공식 Document](https://aka.ms/tsconfig.json){: target="\_blank"}
