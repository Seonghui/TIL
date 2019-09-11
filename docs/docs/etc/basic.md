---
layout: default
title: 자주 까먹는 각종 개념 모음
parent: etc
nav_order: 2
---

# 자주 까먹는 각종 개념 모음

## 1. 보일러플레이트\(Boilerplate\)

쉽게 말해 재사용 가능한 프로그램임. 만약 react.js라는 기술을 사용하려면 이런저런 것들을 설치하고 설정을 건드려야 되는데, 만약 이렇게 하나하나 구축하면 시간이 매우 오래걸림. 따라서 바로 시작할 수 있는 환경을 제공해주는 것이라고 생각하면 됨. 예를 들어 React.js의 경우, create-react-app을 사용하면 쉽게 react.js 개발환경을 구축할 수 있음.

## 2. 컴포넌트

> [참고 링크](https://rinae.dev/posts/why-every-beginner-front-end-developer-should-know-publish-subscribe-pattern-kr), [번역](https://itnext.io/why-every-beginner-front-end-developer-should-know-publish-subscribe-pattern-72a12cd68d44)
>
> * 컴포넌트들은 단일 책임 원칙에 의해 오직 하나의 인터페이스만 담당하는 게 좋음.
> * 컴포넌트를 UI 단위로 나눌 수도 있지만 더 쪼갤 수도 있음 \(기능별, 서비스별, 데이터베이스별, 컴포넌트별...\)

### 파일 분리 예시

지도 api를 사용해서 화면상의 지도에 마커를 표시하고 그 마커의 좌표값을 localstorage에 저장하는 경우, 파일은 아래와 같이 나눌 수 있음.

* dataService.js: 지도 api의 데이터를 localstorage에 저장하거나 꺼냄
* map.js: 맵 컴포넌트 파일로 마커를 마커 배열에 추가
* sidebar.js: 사이드바 컴포넌트 파일

이렇게 파일을 컴포넌트화 시키고 그 서비스 메소드가 컴포넌트가 필요한 서비스\(수신자, subscriber\)가 그 서비스 메소드\(발행자, publisher\)를 실행시키면 됨. \(미쳣다.. 이게 구독이구나.. 이걸 이제 알다니..\) 아무튼 초기 형태의 publish-subscibe 패턴은 한 서비스 메소드가 하나의 데이터 처리 결과를 수신할 수 있어 다른 함수를 넘겨버리면 덮어씌워지게 됨. 이걸 해결하려면 배열로 바꾸어서 함수를 받으면 됨. 현재는 이러한 개념을 기반으로 한 다양한 변종이 있음.

promise는 특정 관점에서 바라보면 우리가 미뤄둔 어떠한 액션이 완료되는 것을 구독할 수 있게 하고, 데이터가 준비되면 발행.

