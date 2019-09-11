---
layout: default
title: URL(Uniform Resource Locator)
parent: http
nav_order: 2
---

# URL\(Uniform Resource Locator\)

### 일반적인 URL의 형식

```text
protocol://computer_name:port/document_name?parameters
```

### 예시

![url structure](https://www.rankwatch.com/learning/sites/default/files/2.jpg)

http 프로토콜을 사용해서 www.mywebsite.com이라는 서버의 접속하고, apparel 경로에 있는 skirt.php에 쿼리 파라메터들이 전달됨.

### Protocol

* 웹에서 페이지나 파일에 접근하는데 사용되는 메소드.
* http, https, ftp, file, mailto 등등이 있음.

### Hostname

* 연결할 파일이 위치한 서버.
* 실제로 통신하는 곳임.
* 서버 주소에는 프로토콜 서비스의 포트 번호가 올 수 있음 \(ex: [http://localhost:8000](http://localhost:8000)\).

### 포트

* 포트는 아파트 등의 현관 우편함 같은 것.
* 같은 주소라도 포트가 다르면 독립적으로 복수의 서버를 운영해 서비스 제공 가능
* 포트가 생략되면 스키마별 기본 포트를 사용
* HTTP는 80, HTTPS는 443번 포트가 기본 포트

### Directory

* 연결할 파일이 들어있는 폴더 디렉토리

### Filename

* 연결되어 보여줄 파일의 실제 이름

### Query parameters

* 파일 이름과 쿼리 사이에 물음표로 구분함. 쿼리 세그먼트는 &로 구분

## refs

* 리얼월드 HTTP

