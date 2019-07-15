# oauth

### OAuth

* Authentication: 인증
* Authorization: 허가

일반 로그인은 사원이 63빌딩에 출입하는 것\(사원증\)이라면, OAuth는 1층에서 방문증을 수령한 후 63빌딩에 출입하는 것.

## OAuth 1.0

### OAuth 1.0의 특징

1. API 인증시 써드파티 어플리케이션에게 사용자의 비번을 노출하지 않고 인증 가능
2. 인증과 권한 부여를 동시에 할 수 있음

### OAuth 1.0의 동작방식

기본적으로 User, Consumer, service provider가 존재

* User: Service Provider에 계정을 가지고 있으면서, Consumer를 이용하려는 사용자
* Consumer: OAuth 인증을 사용해 Service Provider의 기능을 사용하려는 애플리케이션이나 웹 서비스
* Service provider: OAuth를 사용하는 Open API를 제공하는 서비스 \(트위터, 페이스북 등\)

![](https://swalloow.github.io/assets/images/oauth1_triangle.png)

1. 사용자가 트위터 로그인 요청
2. 사용자가 트위터 로그인 화면으로 리다이렉트
3. 트위터 로그인 진행
4. 서비스로 인증토큰\(Access Token\) 전달

### 토큰\(Token\)

#### 리쿼스트 토큰 \(Request Token\)

Consumer가 Service provider에게 접근 권한을 인증받기 위해 사용하는 값. 인증이 완료된 후에는 Access Token으로 교환.

#### 인증 토큰 \(Access Token\)

* 사용자의 아이디, 패스워드를 몰라도 토큰을 통해 허가받은 API에 접근 가능
* 필요한 API에만 제한적으로 접근할 수 있도록 권한 제어 가능
* 저장되어있는 인증토큰이 유출되더라도 트위터의 관리자 화면에서 인증토큰의 권한 취소 가능
* 사용자가 트위터의 패스워드를 변경해도 인증토큰은 계속 유효

## OAuth 2.0

### 1.0에서 바뀐 것

* 용어 번경
* 간단하고 직관적
* 더 많은 인증방법을 지원
* 대형 서비스로의 확장성 지원

## Refs

* [https://d2.naver.com/helloworld/24942](https://d2.naver.com/helloworld/24942)
* [http://earlybird.kr/1584](http://earlybird.kr/1584)

