---
layout: post
title: "크로미움 브라우저에서 명령어 활용하기"
date: 2023-03-15 13:20 +0900
categories: WEB-FRONTEND
permalink: /archivers/chromiumCommand
published: true
nocomments: false
use_math: true
---

# Run commands in the Chromium Command Menu

|    ![memoryEater](/assets/posts/2023-03-15-chromeCommands/memoryeatchrome.gif)     |
| :-----------------------------------------------------------------: |
| <b>Run commands in the Chromium Command Menu</b> |

## Quick Refresh

크로미움 브라우저의 개발자 도구는 *HTML*, *CSS*, *JavaScript* 등으로 이루어진 웹 개발에 관련된 여러 다양한 기능을 제공한다

그러한 다양한 기능을 접근할 때 개발자 도구의 UI가 아닌 콘솔 명령어를 통해 접근할 수 있는데, 이를 알아두면 빠르게 원하는 기능에 접근할 수 있다.

개발자 도구는 *F12* 및 *Ctrl(command) + Shift + I* 의 단축키로 접근할 수 있고, 명령어 기능은 개발자 도구에 진입 후 *ctrl(command) + shift + p* 를 통해 접근한다.

해당 포스트는 개발자 도구에서 제공하는 몇몇 유용한 <b>명령어 기능</b>에 대해 알아본다.

## 사라지는 HTML 요소 디버깅하기

+ `Emulate a focused page` : 아래의 사례와 같은 사라지는 HTML 요소를 디버깅 할 때 활용

![01](/assets/posts/2023-03-15-chromeCommands/first.gif)

웹 페이지에는 위의 사례와 같이 *AutoComplete* 필드나 커서 위치에 따라 나타나거나 사라지는 HTML 요소들이 있다.

이러한 요소를 개발자 도구를 통해 검사하고자 우클릭으로 검사 버튼을 클릭하면, 클릭한 순간 `focus` 가 해제되어버려 해당 HTML 요소를 디버깅할 수 없어진다.

![02](/assets/posts/2023-03-15-chromeCommands/second.gif)

이 때, 아래와 같이 명령어 콘솔에 `focus` 를 입력해주면, `Emulate a focused page` 옵션이 등장한다.

![EmulateAfocusedPage](/assets/posts/2023-03-15-chromeCommands/focus.png)

이렇게 한 후에, 다시 검사를 시도하면 아래와 같이 사라지지 않고 디버깅이 가능해진다.

![03](/assets/posts/2023-03-15-chromeCommands/third.gif)

## Capture

웹 페이지를 이미지로 캡쳐해주는 기능을 실행하는 명령어들이다.

*ctrl(command) + shift + m* 등으로 *Responsive* 화면 설정을 한 이후 실행하면 모바일이나 태블릿 화면 등도 캡쳐할 수 있다.

+ `Capture area screenshot` : 마우스 커서가 십자모양으로 바뀌며, 웹 페이지에서 드래그한 부분만 캡쳐하여 저장한다.
+ `Capture full size screenshot` : 현재 화면에 보이지 않는 *x, y축 스크롤* 부분 및 웹 페이지 UI의 전부를 캡쳐하여 저장한다.
+ `Capture node screenshot` : *HTML node Element*를 선택하고 실행하면, 해당 노드 부분만을 캡쳐하여 저장한다.
+ `Capture screenshot` : 보고있는 화면 그대로를 캡쳐하며, *스크롤바* 가 있는 경우, *스크롤바* 의 모습도 캡쳐된다.

## miscellaneous

이하 부터는 기타 재밌는 기능 등을 서술

### Emulate vision deficiencies

+ `Emulate blurred vision` : HTML 요소들을 모두 블러 처리하여 보여줌
+ `Emulate protanopia (no red)` : HTML 요소에서 적색을 제거함 (UI 디자인 시 색맹을 고려할 때 사용)
+ `Emulate tritanopia (no blue)` : HTML 요소에서 청색을 제거함
+ `Emulate deuteranopia (no green)` : HTML 요소에서 녹색을 제거함
+ `Emulate achromatopsia (no color)` : HTML 요소에서 색상을 제거하고 흑백으로 표현

### Developer tool Theme Toggle

+ `Switch to light theme` : 개발자 도구의 테마를 라이트 테마로 변경
+ `Switch to dark theme` : 개발자 도구의 테마를 다크 테마로 변경

### Open File

개발자 도구에서 *ctrl(command) + shift + p* 가 아닌, *ctrl(command) + p* 를 입력하고 웹페이지를 구성하는 파일(*.html*, *.css*, *.js* 등의 파일)의 파일명을 입력하면 해당 *file* 을 바로 열 수 있다.

![openFile](/assets/posts/2023-03-15-chromeCommands/openfile.gif)

## References

[? Chrome Developers 문서 해당항목](https://developer.chrome.com/docs/devtools/command-menu/)