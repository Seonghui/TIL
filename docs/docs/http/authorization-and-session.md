---
layout: default
title: 인증과 세션
parent: http
nav_order: 2
---

# 인증과 세션

## 1. BASIC 인증과 Digest 인증

### 1-1. BASIC 인증

* BASIC 인증은 유저명과 패스워드를 BASE64로 인코딩한 것.
* BASE64 인코딩은 가역변환이므로 서버로부터 복원해 원래 유저명과 패스워드를 추출할 수 있다.
* 추출된 정보를 서버의 데이터베이스와 비교해서 정상 사용자인지 검증
* 단 SSL/TLS 통신을 사용하지 않은 사앹에서 통신이 감청되면 손쉽게 로그인 정보가 유출된다

### 1-2. Digest 인증

* BASIC 인증보다 더 강력한 방식
* 해시 함수\(A-&gt;는 쉽게 계산할 수 있지만, B-&gt;A는 쉽게 계산할 수 없다\)를 이용한다.

### 1-3. 쿠키를 이용한 세션 관리

지금은 BASIC 인증과 Digest 인증 모두 많이 사용되지 않는다. 이유는 아래와 같다.

* 특정 폴더 아래를 보여주지 않는 방식으로만 사용할 수 있어, 톱페이지에 사용자 고유 정보를 제공할 수 없다. 톱페이지에 사용자 고유 정보를 제공하려면 톱페이지도 보호할 필요가 있어, 톱페이지 접속과 동시에 로그인 창을 표시해야 한다.
* 요청할 때마다 유저명과 패스워드를 보내고 계산해서 인증할 필요가 있다. 특히 Digest 인증 방식은 계산량도 많다.
* 로그인 화면을 사용자화할 수 없다.
* 명시적인 로그오프를 할 수 없다. \(왜..?\)
* 로그인한 단말을 식별할 수 없다. 게임 등 동시 로그인을 막고 싶은 서비스나 구글처럼 미등록 단말로 로그인할 때 보안 경고를 등록된 메일로 보내는 기능이 있는 웹서비스도 있다.

## 2. 쿠키를 사용한 세션 관리

최근 많이 사용되는 방식은 폼을 이용한 로그인과 쿠키를 이용한 세션 관리의 조합이다.

1. 클라이언트는 폼으로 ID와 비밀번호를 전송한다. Digest 인증과 달리, 유저 ID와 패스워드를 직접 송신하므로 SSL/TLS이 필수이다.
2. 서버측에서 유저 ID 와 패스워드로 인증하고 문제가 없으면 세션 토큰을 발행한다.
3. 서버는 세션 토큰을 관계형 데이터베이스나 키 밸류형 데이터베이스에 저장해둔다.
4. 토큰은 쿠키로 클라이언트에 되돌아간다.
5. 두 번째 이후 재접속에서는 쿠키를 재전송해서 로그인된 클라이언트임을 서버가 알 수 있다.
