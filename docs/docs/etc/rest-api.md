---
layout: default
title: REST API
parent: etc
nav_order: 2
---

# REST API

REST는 Representational State Transfer라는 용어의 약자로서 2000년도에 로이 필딩 (Roy Fielding)의 박사학위 논문에서 최초로 소개되었다. 로이 필딩은 HTTP의 주요 저자 중 한 사람으로 그 당시 웹(HTTP) 설계의 우수성에 비해 제대로 사용되어지지 못하는 모습에 안타까워하며 웹의 장점을 최대한 활용할 수 있는 아키텍처로써 REST를 발표했다고 한다.

여기에 ~ful 이라는 형용사형 어미를 붙여 ~한 API 라는 표현으로 사용된다. 즉, REST 의 기본 원칙을 성실히 지킨 서비스 디자인은 'RESTful'하다라고 표현할 수 있다.

REST가 디자인 패턴이다, 아키텍처다 많은 이야기가 존재하는데, 하나의 아키텍처로 볼 수 있다. 좀 더 정확한 표현으로 말하자면, REST 는 Resource Oriented Architecture 이다. API 설계의 중심에 자원(Resource)이 있고 HTTP Method 를 통해 자원을 처리하도록 설계하는 것이다.

## REST의 구성

1. 자원(RESOURCE) - URI
2. 행위(Verb) - HTTP METHOD
3. 표현(Representations)

## REST 의 특징

### Uniform (유니폼 인터페이스)

Uniform Interface는 URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일을 말합니다.

### Stateless (무상태성)

REST는 무상태성 성격을 갖습니다. 다시 말해 작업을 위한 상태정보를 따로 저장하고 관리하지 않습니다. 세션 정보나 쿠키정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 됩니다. 때문에 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해집니다.

### Cacheable (캐시 가능)

REST의 가장 큰 특징 중 하나는 HTTP라는 기존 웹표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용이 가능합니다. 따라서 HTTP가 가진 캐싱 기능이 적용 가능합니다. HTTP 프로토콜 표준에서 사용하는 Last-Modified태그나 E-Tag를 이용하면 캐싱 구현이 가능합니다.

### Self-descriptiveness (자체 표현 구조)

REST의 또 다른 큰 특징 중 하나는 REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조로 되어 있다는 것입니다.

### Client - Server 구조

REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 됩니다.

### 계층형 구조

REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 합니다.

## REST API 디자인 가이드

1. URI는 정보의 자원을 표현해야 한다.
2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.

```text
GET /members/delete/1
```

위와 같은 방식은 REST를 제대로 적용하지 않은 URL이다. URL은 자원을 표현하는데 중점을 두어야 한다. delete와 같은 행위에 대힌 표현이 들어가서는 안 된다.

```text
DELETE /members/1
```

자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)로 표현해야 한다.

### 예시

* 회원정보를 가져오는 URL

```text
GET /members/show/1     (x)
GET /members/1          (o)
```

* 회원을 추가할 때

```text
GET /members/insert/2 (x)  - GET 메서드는 리소스 생성에 맞지 않습니다.
POST /members/2       (o)
```

## HTTP Method

POST, GET, PUT, DELETE 이 4가지의 Method를 가지고 CRUD를 할 수 있다.

| Method | 역할 |
| :--- | :--- |
| POST | POST를 통해 해당 URI를 요청하면 리소스를 생성. |
| GET | GET를 통해 해당 리소스를 조회. 리소스를 조회하고 해당 도큐먼트에 대한 자세한 정보를 가져온다. |
| PUT | PUT를 통해 해당 리소스를 수정. |
| DELETE | DELETE를 통해 리소스를 삭제. |

다음과 같은 식으로 URI는 자원을 표현하는 데에 집중하고 행위에 대한 정의는 HTTP METHOD를 통해 하는 것이 REST한 API를 설계하는 중심 규칙이다.

## REST API의 장단점

### 장점

1. Open API 를 제공하기 쉽다
2. 멀티플랫폼 지원 및 연동이 용이하다.
3. 원하는 타입으로 데이터를 주고 받을 수 있다.
4. 기존 웹 인프라(HTTP)를 그대로 사용할 수 있다.

### 단점

1. 사용할 수 있는 메소드가 4 가지 밖에 없다.
2. 분산환경에는 부적합하다.
3. HTTP 통신 모델에 대해서만 지원한다.

## References

* [REST API 제대로 알고 사용하기](https://meetup.toast.com/posts/92)
* [Interview Question for Beginner](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Development_common_sense#restful-api)
