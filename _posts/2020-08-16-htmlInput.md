---
layout: post
title: "HTML Input tag 특정 값만 받기"
date: 2020-08-16 15:30:00
categories: WEB-FRONTEND
permalink: /archivers/HTMLInput
nocomments: false
use_math: true
---

# HTML Input tag 특정 값만 받기

한글 / 영어 / 숫자 / 소수점 등 input tag에 특정 문자열만 입력받을 수 있게 설정

[keyCode이용, 정규표현식 이용 등(클릭 시 새 탭 열기)](https://m.blog.naver.com/PostView.nhn?blogId=deeperain&logNo=221487343491&proxyReferer=https:%2F%2Fwww.google.com%2F%27){: target="\_blank"}

[KeyCode 229에 관하여 (클릭 시 새 탭 열기)](https://circus7.tistory.com/6){: target="\_blank"}

```javascript
ime-mode:disabled
```

위의 방법은 Chrome에서 지원하지 않음

## onKeyPress

```javascript
// input에 숫자만 전달하도록 하는 함수
function inNumber() {
  if (!(event.keyCode == 46 || (event.keyCode > 47 && event.keyCode < 58))) {
    event.returnValue = false;
  }
}
```

## onkeyup

<!-- ![url](/assets/posts/2020-05-24-nodejs/url.png)

[URL structure (클릭 시 새 탭 열기)](https://howurls.work/#/){: target="\_blank"} -->

onkeyup

## onkeydown

onkeydown
