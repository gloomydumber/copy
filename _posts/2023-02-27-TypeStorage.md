---
layout: post
title: "Creating LocalStorage Wrapper With TypeScript"
date: 2023-02-28 13:40 +0900
categories: WEB-BACKEND
permalink: /archivers/typeStorage
nocomments: false
use_math: true
---

# Creating LocalStorage Wrapper With TypeScript

|    ![wrapDog](/assets/posts/2023-02-27-TypeStorage/wrapdog.gif)     |
| :-----------------------------------------------------------------: |
| <b>Imporve the usage of browser APIs by adding a custom wrapper</b> |

이 내용은 Medium 원문의 _Creating LocalStorage Wrapper With TypeScript_ 등을 참조하여 작성하였다.

[🔗 Medium 원문](https://betterprogramming.pub/creating-localstorage-wrapper-with-typescript-7ff6b71b35cb)

## Quick Refresh

`localStorage` 는 브라우저 세션을 통해 데이터를 저장하는 브라우저 스토리지이다. `localStorage` 는 간단한 인터페이스를 갖추고 있기 때문에, 쉽게 다룰 수 있다.

{% gist 44af3337a1183f0ef7d9cb5d6fdcfb08 %}

쉬운 인터페이스를 갖추고 있는데, 왜 다시 *wrapper* 처리를 해서 복잡하게 만드냐고 질문할 수 있다. *wrapper* 처리는 간단한 사용성 이외에도 개발 과정에 있을 여러 다른 부분을 도울 수 있다.

아래의 목록이 `localSotrage` 의 *wrapper* 처리를 통해 얻을 수 있는 이점이다.

## Advantages

+ 테스트의 도입을 더욱 쉽게 해준다. *wrapper* 를 통해 추상화가 잘 된 API는 테스트에 용이하다.
+ `key` 들의 이름을 관리할 단일 공간을 갖게 해준다. 이는 존재하지 않는 키 이름이나 다른 키에 `value` 를 저장하지 않도록 방지해준다.
+ `key` 의 이름을 변경할 필요가 있을 경우, *wrapper* 를 통해 변경할 수 있다.
+ 코드 관리에 용이하다.

## Developing

우선, `generic` 과 함께 추상화 된 `class`를 선언한다.

{% gist eada12f68308897c8c25b7022c4b1353 %}

실제 `storage` 의 기능을 갖춘 `property`를 추가 해야 한다. 그 전에, 해당 기능을 추상화해준다.

{% gist 0f976ef1f4ce312aacda84b2872bff3d %}

위 코드와 같이, 브라우저에서 제공하는 `localStorage` 와 같은 기능을 다시 작성한 것 뿐이다.

아래는 `property`를 갖춘 추상화 `class` 코드이다.

{% gist e0e0f7f8d5399b05f72b40a5b652eae4 %}

`getStorage` 메소드를 이용하여, 브라우저의 `localStorage`를 반환하는 실제 스토리지를 다룰 수 있다.

{% gist 7df69b5c64630844316960bc32fc1577 %}

모든 메소드에 대해 외부 클래스나 인스턴스에서 이용할 일이 없으므로, 모든 메소드를 `protected` 접근 제한자를 이용한다.

우선, `get` 메소드에 대해,

{% gist 56734e9015fe185034f85de6a9461413 %}

`T` 제네릭을 이용하여, 추상화 클래스를 다룰 수 있다.

`getItem` 이 `string`이 아닌 `null` 을 반환할 수 도 있으므로, `string` 과 함께 명기해준다.

`set` 메소드는 이하와 같다.

{% gist 7c89d493052dbdce9bfacec250a9cc0f %}

또, `clearItem`과 `clearItems` 메소드를 통해 단일 `key` 값이나 여러 `key` 값을 제거하는 기능을 추상해둔다.

{% gist 1950e820f8214eee276c368145425903 %}

여태까지 추상화한 모든 코드는 아래와 같다.

{% gist e41ad18994f381b8d4060021e5d437ed %}

모든 추상화가 위와 같이 끝났으므로, `instance`를 생성해야 한다. 그 전에, 예시를 위해 `enum` 을 통해 `token` 들을 나타내보고자 한다.

{% gist 582328f8f60ddc726b0d8e7b4c8e8773 %}

위 코드를 보면, `Locals` 라는 `enum` 타입만이 `Storage`의 제네릭으로 전달되었다. 오직 `enum`에 있는 값들만 `key` 의 이름으로 처리하여 `get`, `set`, `clearItem`, `clearItems` 등의 메소드를 처리할 수 있도록 제한한 것이다.

이것으로 깔끔한 코드 관리와 추상화를 이끌어낸 `singleton` 디자인을 만족했다고 할 수 있다.

이제 `token`의 사례에서 실제 데이터를 다루는 부분을 구현하면 아래와 같다.

{% gist c2c31c1fedaed2654c2b595b5c2bcf71 %}

마찬가지로 `protected` 접근 제한자를 통해 외부의 접근을 차단한다.

이어서, 아래는 토큰을 리프레쉬하는 부분을 구현한 것이다.

{% gist 9421a64fdd24f93527ce36ef099d5cc0 %}

아래는 토큰을 삭제하는 부분을 구현한 것이다.

{% gist 11cbed099c4ba6ea3fd8cdc0ac9919c3 %}

최종 코드는 아래와 같다

{% gist fc8d62f1a9f6ce0189a96b637cc2106f %}

Usage :

{% gist a8cfb47e676d928af4b8c6d16c1786a1 %}

## Summary

모든 프로젝트가 이러한 접근을 통해 모두 `localStorage` 기능을 추상화하여 `singleton` 관리를 필요로하지는 않지만, 분명 필요로하는 곳이 있고 알아두어야만 한 부분이다.

여러 프로젝트 진행한 경험에 비추어 보면 `javascript`를 이용한 `localStorage` 의 관리가 지저분해지는 경우가 많았으므로 자주 사용할 것 같다.