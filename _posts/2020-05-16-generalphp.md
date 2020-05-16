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

CGI (Common Gateway Interface) : Web Server와 (PHP Engine) 사이의 통신 규약

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

<!-- ![executeCMD](/assets/posts/2020-03-21-bithumbcal/bithumbcal.gif)

Need optimization and more functions -->
