---
layout: default
title: 캐시
parent: http
nav_order: 2
---

# 캐시

* 콘텐츠가 변경되지 않았을 땐 로컬에 저장된 파일을 재사용함으로써 다운로드 횟수를 줄이고 성능을 높인다.
* GET, HEAD 메서드 이외에는 기본적으로 캐시되지 않는다.

## 갱신 일자에 따른 캐시

웹 서버는 대개 아래와 같은 헤더를 응답에 포함한다.

```text
Last-Modified: Wed, 08 Jun 2016 15:23:45 GTM
```

웹 브라우저가 캐시된 URL을 읽을 때는 서버에서 반환된 일시를 그대로 If-Modified-Since 헤더에 넣어 요청한다.

```text
If-Modified-Since: Wed, 08 Jun 2016 15:23:45 GTM
```

웹 서버는 요청에 포함된 If-Modified-Since 일시와 서버의 콘텐츠의 일시를 비교한다. 변경되었으면 정상 스테이터스 코드 200 OK를 반환하고 콘텐츠를 응답 바디에 실어서 보낸다. 변경되지 않아았으면, 스테이터스 코드 304 Not Modified를 반환하고 바디를 응답에 포함하지 않는다.

## Expires

갱신 일시를 이용하는 캐시의 경우 캐시의 유효성을 확인하기 위해 통신이 발생한다. 그런데 Expires 헤더를 이용하면 이 통신 자체를 없앨 수 있다.

Expires 헤더에는 날짜와 시간이 들어간다. 클라이언트는 지정한 기한 내라면 캐시가 신선하다고 판단해 강제로 캐시를 이용한다. 다시 말해 요청을 아예 전송하지 않는다. 캐시의 유효 기간이 지났으면 캐시가 신선하지 않다고 판단한다.

```text
Expires: Wed, 08 Jun 2016 15:23:45 GTM
```

## Pragma: no-cache

Pragma 헤더를 이용하면 클라이언트가 프록시 서버에 지시를 할 수 있다. Pragma 헤더에 포함할 수 있는 유일한 사양은 no-cache이다.

no-cache는 요청한 콘텐츠가 이미 저장돼 있어도, 원래 서버\(오리진 서버\)에서 가져오라고 프록시 서버에 지시하는 것이다. no-cache는 Cache-Control로 통합됐지만 하위 호환성 유지를 위해 남아있다.

## Cache Control

Cache-Control헤더로 더 유연한 케시 제어를 지시할 수 있다. Expires보다 우선해서 처리한다. 서버가 응답으로 보내는 헤더의 키는 아래와 같다.

* **public**: 같은 컴퓨터를 복수의 사용자간 캐시 재사용을 허가한다.
* **private**: 같은 컴퓨터를 사용하는 다른 사용자 간 캐시를 재사용하지 않는다. 같은 URL에서 사용자마자 다른 콘텐츠가 돌아오는 경우에 이용한다.
* **max-age=n**: 캐시의 신선도를 초단위로 설정. 86400을 지정하면 하루동안 캐시가 유효하고 서버에 문의하지 않고 캐시를 이용한다. 그 이후 서버의 문의한 뒤 304 Not Modified가 반환되었을 때만 캐시를 이용한다.
* **s-maxage=n**: max-age와 같으나 공유 캐시에 대한 설정값이다.
* **no-cache**: 캐시가 유효한지 매번 문의한다. max-age=0와 거의 같다.
* **no-store**: 캐시하지 않는다.

no-cache는 Pragma: no-cache와 똑같이 캐시하지 않는 것은 아니고, 시간을 보고 서버에 접속하지 않은 채 콘텐츠를 재이용하는 것을 그만둘 뿐이다.

## Vary

같은 URL이라도 클라이언트에 따라 반환 결과가 다름을 나타내는 헤더이다. 사용자의 브라우저가 스마트폰용 브라우저일 때는 모바일용 페이지가 표시되고, 사용하는 언어에 따라 내용이 바뀌는 경우가 그 예이다. 이렇게 표시가 바뀐느 이유에 해당하는 헤더명을 Vary에 나열함으로써 잘못된 콘텐츠의 캐시로 사용되지 않게 한다.

```text
Vary: User-Agent, Accept-Language
```

## Referer

사용자가 어느 경로로 웹사이트에 도달했는지 서버가 파악할 수 있도록 클라이언트가 서버에 보내는 헤더이다.

```text
referer: https://www.google.co.kr/
```

만약 북마크에서 선택하거나 주소창에서 키보드로 직접 입력했을 때는 Referer태그를 전송하지 않더나 Referer:ablut:blank를 전송한다. 기본 동작으로는 https -&gt; http 일때 전송하지 않는 것이 기본이다.

### 리퍼러 정책으로 설정할 수 있는 값

* **no-referrer**: 전혀 보내지 않는다
* **no-referrer-when-downgrade**: 현재 기본 동작과 마찬가지로 https -&gt; http일때는 전송하지 않는다
* **same-origin**: 동일 도메인 내의 링크에 대해서만 리퍼러를 전송한다
* **origin**: 상세페이지가 아니라 톱페이지에서 링크된 것으로 해 도메인 이름만 전송한다 \(무슨말이지\)
* **strict-origin**: origin과 같지만 https -&gt; http일때는 전송하지 않는다
* **origin-when-crossorigin**: 같은 도매인 내에서는 완전 리퍼러\(?\)를, 다른 도메인에서는 도메인 이름만 전송한다.
* **strict-origin-when-crossorigin**: origin-when-crossorigin과 같지만 https-&gt;http일때는 송신하지 않는다.
* **unsafe-url**: 항상 전송한다

## refs

* 리얼월드 HTTP

