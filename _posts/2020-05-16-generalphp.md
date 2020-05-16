---
layout: post
title: "PHP"
date: 2020-05-16 12:45:00
categories: WEB-BACKEND
permalink: /archivers/php
nocomments: false
use_math: true
---

# PHP

PHP에 관해 공부하며 간단 메모한 글

## What is PHP?

Server Side Script

Server Side Technology

Web Browser(=Web Client)에 대응하는 Web Server를 구동하는 Serverside Script Language

주로 HTML 코드를 프로그래밍적으로 생성

1995 라스무스 러도프가 개발

At first, stands for Personal HomePage tools

Now, stands for Hypertext PreProcessor

초창기 Perl Language로 작성 후 C Language로 다시 작성

대부분의 Web Hosting 환경에서 사용 가능

Interpreter 방식의 언어

## Why Server-side Script

Web-Server만으로 정적인 HTML을 처리하는 데에 한계가 있음

CGI (Common Gateway Interface) : Web Server와 (PHP Engine) 사이의 통신 규약

을 통해 php, java, python, perl 등의 script 언어로 정적인 HTML을 동적으로 처리할 수 있게 함

## Web Servers's example

Apache / IIS / ngnix

## APM

Apache PHP MySQL

with Windows -> WAPM (Bitnami로 한번에 구성 가능)

## PHP's Setting File

php.ini

php 설정

php의 idle 설정으로 error 출력 기능이 꺼져 있다.

그 이유는 보안 상 error가 출력 되면 어떤 부분이 약점인지 드러날 수 있기 때문이다.

따라서 개발 환경에서는 error 출력 기능을 켜고, 실제 서버를 구동할 때에는 꺼야한다.

이를 위해 php는 다음과 같이 두 가지 configuration file을 제공한다.

php.ini-development : 개발 환경

php.ini-production : 실 서버 환경

php.ini의 다음과 같은 코드 수정으로 error 출력 기능을 켤 수 있다.

```php
display_errors = Off
```

```php
display_errors = On
```

error.log 파일에, error log가 기록되며, php.ini에서 기록기능을 on/off 할 수 있다.

```php
log_errors = On
```

개발환경에서의 추천되는 php.ini 설정

```php
display_errors = On
display_startup_errors = On
error_reporting = -1
log_errors = On
```

실 서버 운영환경에서의 추천되는 php.ini 설정

```php
display_errors = Off
display_startup_errors = Off
error_reporting = E_ALL
log_errors = On
```

## GET VS. POST

GET 방식과 POST 방식의 특징 및 차이점

### GET

URL상에 서버로 전송하는 데이터가 포함

정보 등이 URL이 포함 되어야 할 때 사용

### POST

GET 방식에 비해 보안성 높음

ID / Password 등을 처리할 때 사용

HTTP 프로토콜을 통해 송수신 데이터가 처리 (Fiddler등의 유틸리티를 통해 Server-CLient간 송수신 내용 확인)

## 문법 간단 메모

문자열을 합할 땐 + 가 아닌, .을 사용

변수 앞에 \$를 붙여서 표기 및 선언

echo = print

define : 상수화 함수, 상수는 대문자로 정의하는 것이 관습

```php
define('TITLE', 'PHP Tutorial'); # TITLE을 PHP Tutorial 이라는 문자열을 갖는 상수 취급
echo TITLE; # PHP Tutorial 출력
define('TITLE', 'JAVA Tutorial'); # 오류, 이미 정의된 TITLE을 재정의 불가
```

var_dump : 데이터 형 출력

gettype : 데이터 형 검사

settype : 데이터 형 변환

is_array / is_bool / is_callable / is_double / is_int 등의 API

가변변수 (Variable Variables) : 변수의 이름을 변수로 변경

```php
$title = 'subject';
$$title = 'PHP Tutorial';
echo $subject; #PHP Tutorial 출력
```

주소의 ? 이후에는 값 전달

& 는 값과 값사이의 구분자로 값 전달

ID / Password 를 서버 송수신의 간단 예 코드

```html
<html>
  <body>
    <form method="get" action="2.php">
      id : <input type="text" name="id" /> password :
      <input type="text" name="password" />
      <input type="submit" />
    </form>
  </body>
</html>
```

논리 연산자 and = && or = \|\| 양쪽 모두 사용 가능

gettype() empty() is_null() isset() 모두 Case별로 값이 다름

== (loose comparison) === (strict comparison)

[PHP type table Document (클릭 시 새 탭 열기)](http://php.net/manual/en/types.comparisons.php){: target="\_blank"}

인자의 기본값은 아래와 같이 작성

```php
<?php
function get_arguments($arg1=100){
    return $arg1;
}
echo get_arguments(1);
echo ',';
echo get_arguments();
?>
```

변수 선언 및 이용은 아래와 같이 작성

```php
<?php
$member = ['leaver', 'leaveroov', 'gloomydumber']; # php 5.4 upper version
?>
```

```php
<?php
$member = array('leaver', 'leaveroov', 'gloomydumber'); # php 5.4 under version
?>
```

count : 배열 안의 요소 숫자 반환하는 함수

ucfirst : 첫 번째 글자를 대문자로 반환하는 함수

사용 예 코드

```php
<?php
function get_members(){
    return ['leaver', 'leaveroov', 'gloomydumber'];
}

$members = get_members();

for($i = 0; $i < count($members); $i++){
    echo ucfirst($members[$i]).'<br />';
}

?>
```

array_push / array_merge / array_unshift / array_splice / array_shift 등으로 배열 조작 가능

sort / rsort 등으로 배열 정렬 가능

연관배열(associative array, indexed array) : key, value 값을 가지는 배열, python의 dictrionary

연관배열에서는 for문이 아닌 foreach 라는 반복문 사용

```php
<?php
$grades = array('leaver'=>10, 'leaveroov'=>6, 'gloomydumber'=>80);
foreach($grades as $key => $value){
    echo "key: {$key} value:{$value}<br />";
}
?>
```

include / include_once / require / require_once 로 다른 code 및 file을 load하여 사용

namespace의 이용

```php
<?php
namespace language\en;
function welcome(){
    return 'Hello world';
}
namespace language\ko;
function welcome(){
    return '안녕세계';
}
```

```php
<?php
require_once 'greeting_lang.php';
echo language\ko\welcome();
echo language\en\welcome();
?>
```

[PHP 표준 Library (클릭 시 새 탭 열기)](https://www.php.net/manual/en/){: target="\_blank"}

Composer : 의존성 관리 도구

copy 함수를 통해 파일 간 복사 가능, 반환 값은 복사 성공 / 실패로 True/False 반환

unlink : 파일 삭제 함수

file_get_contents / file_put_contents 파일 읽기/쓰기 함수

file_get_contents는 URL로 불러올수도 있음, 다만 http://.../xx.php를 불러 왔다고 가정하면, css/javascript는 불러와지지 않았기 때문에 온전한 상태의 웹페이지가 불러와지진 않는다.

fopen 등으로 파일을 더욱 정교하게 처리

트러블슈팅 : 파일을 읽고 쓸 때 권한의 문제 등을 처리

<!-- ![executeCMD](/assets/posts/2020-03-21-bithumbcal/bithumbcal.gif)

Need
URI
URL
HTTP
 -->
