---
layout: post
title: "jekyll 블로그 이용 시 새로운 포스트가 나타나지 않을 때"
date: 2023-04-22 10:40 +0900
categories: ETC
permalink: /archivers/jekyllNewpost
nocomments: false
use_math: true
---

# Jekyll post not generated

|    ![writing](/assets/posts/2023-04-22-jekyllnewpost/writing.gif)     |
| :-----------------------------------------------------------------: |
| <b>what makes jekyll post generating</b> |

## Quick Refresh

개발 블로그를 운영하기위해 여러 플랫폼이나 방법이 있지만, *github-pages*와 *jekyll*를 활용하는 방법을 사용하고 있다.

*github-pages* 를 이용하면 *jekyll* 이나 *gatsby* 등을 활용하여 커스텀 가능한 블로그를 꾸미거나, 자신의 포스트 데이터를 비교적 본인 스스로가 관리할 수 있다.

다른 플랫폼에 글을 올렸을 때에 비해 자신의 컨텐츠가 중앙화 되는 문제가 상대적으로 덜 하다.

하지만 단점도 있는데, *github-pages* 공식 명세에 따르면 *github-pages* 를 활용한 사이트의 경우 *1GB*의 용량을 넘길 수 없고 트래픽도 한 달에 *100GB*로 제한된다.

이에 이미지는 블로그 크기에 따라 다른 클라우드 CDN을 이용하는 등의 차선책이 필요할 수도 있다.

또 이 포스트에서 다루는 *jekyll* 상의 명세를 잘 지키지 않을 경우에도 문제가 발생할 수 있을 것이다.

## Jekyll Creating Posts

*jekyll* 블로그에서 새로운 포스트를 작성하기 위해서는 `_posts` 폴더 아래에 이하와 같은 포맷으로 파일을 작성한다.

> YEAR-MONTH-DAY-title.MARKUP

`YEAR` 는 4자리의 숫자이며, `MONTH` 와 `DAY` 는 모두 2자리의 숫자 형식이여야한다. `MARKUP` 은 확장자이다.

아래는 대표적인 블로그 포스트 파일 형식의 예시이다.

> 2012-09-12-how-to-write-a-blog.md

모든 블로그 포스트는 파일 최상단에 [🔗 `front matter`](https://jekyllrb.com/docs/front-matter/) 형식을 갖추고 있어야 한다.

아래는 `front matter` 를 갖춘 포스트의 예시이다.

{% gist 7c30bb39a626cb7794f16a87f05b5c19521dd796 %}

## `date` Option

앞서 언급한 `front matter` 에서 포스트 작성일을 `date` 필드에 설정해주게 되는데, 이 때 `_config.yml` 에서 설정한 `timezone` 필드를 고려하여 작성해야한다.

가령, 지금이 한국 시간으로 2023년 4월 20일 오전 10시라고 가정해보자

한국의 경우 *UTC+9* 의 시간대를 적용받는데, `_config.yml` 의 `timezone` 필드가 `timezone: Asia/Seoul` 와 같이 설정되지 않았음에도 포스트의 `date` 필드에 `date: 2023-04-20 10:00:00` 라고 작성할 경우에 포스트가 노출이 되지 않는다. 따라서 `timezone`을 명기해주거나 `_config.yml` 의 설정을 고려하여 포스트에도 `date` 필드를 표기해주어야한다.

*jekyll* 공식 명세에서는 `date` 필드를 `YYYY-MM-DD HH:MM:SS +/-TTTT` 포맷으로 작성하고, 시, 분, 초와 타임존 오프셋의 경우 *optional* 하게 부여할 수 있도록 한다.

정리하자면, `2023-04-20 10:00 +0900` 도 가능한 예시이고, `2023-04-20T10:00:00Z` 나 `2023-04-20 10:00:00` 등의 형식 모두 가능하다.

따라서, `date` 필드는 `timezone` 설정을 고려하여 자신의 블로그에서 표기하는 정도를 고려하여 입력하면 된다.

## `future` Option

앞서 언급한 `date` 포맷에 현재 시각보다 미래의 시각을 작성하면 포스트가 보이지 않을 수 있기 때문에 `timezone` 설정과 함께 잘 고려해야할 사항이다

`_config.yml`에 `future` 옵션을 `true` 값을 설정하면 현재 시각보다 미래의 시각으로 설정한 포스트도 보이게 할 수도 있다

## `published` Option

`front matter` 에 `published: false` 옵션을 부여하면 포스트가 노출이 되지 않도록 할 수 있다

혹시나 다른 비공개 포스트의 `front matter` 를 복사하여 사용한 경우가 있을 수 있으니 주의 해야한다

## : 문자

포스트의 제목에 `:` 문자가 포함된 경우 새로운 포스트가 생성되지 않는데, `&#58` 과 같은 `HTML Entity` 형식으로 입력하면 잘 동작한다

## UTF-8 Format

![jekyll-utf8-problem](/assets/posts/2023-04-22-jekyllnewpost/jekyllutf8.png)

상기 이미지는 *github-pages* 를 통해 `jekyll` 을 빌드할 때 발생한 에러 로그이다

포스트 파일이 `utf-8` 포맷으로 작성되지 않으면 이미지와 같이 `invaild byte sequence in UTF-8` 과 같은 에러를 표출한다

*vs code* 에서 *C/C++* 언어 작성시에 한글을 이용하려고 `EUC-KR` 포맷으로 작성하던 설정이 포스트 파일 포맷에도 반영되어 발견한 현상이다

## Summary

새로운 포스트를 작성했음에도 *github-pages* 에 반영이 되지 않을 경우 아래 사항을 체크한다

+ `_post` 디렉토리에 저장했는지 확인 할 것
+ `2012-09-12-how-to-write-a-blog.md` 와 같이 파일 이름 및 확장자 형식을 확실히 할 것
+ `timezone` 을 고려한 `date` 를 지정할 것
+ 지정한 `date` 가 현재 시각에 비해 미래 시각이 아닌 것을 확인 할 것
+ `published` 옵션이 `false` 로 설정하지 않았는지 확인 할 것
+ `:` 와 같은 문자가 제목에 포함되지 않도록 할 것
+ `utf-8` 형식으로 포스트를 작성했는지 확인 할 것

## References

[🔗 jekyll 공식 명세 - Posts](https://jekyllrb.com/docs/posts/)

[🔗 Github Pages 공식 명세 - Usage limits](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#usage-limits)

[🔗 Github-Pages 용량, 트래픽 해결을 위한 원드라이브 이미지 호스팅에 관한 포스트](https://pioneergu.github.io/posts/jekyll-github-blog-image-hosting/)

[🔗 관련 stackoverflow 질문](https://stackoverflow.com/questions/30625044/jekyll-post-not-generated?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)

[🔗 관련 post](https://thecodinglog.github.io/ruby/2018/05/25/jekyll.html)