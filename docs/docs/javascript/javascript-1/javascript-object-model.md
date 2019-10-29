---
layout: default
title: 자바스크립트 구성 요소
parent: 1. 자바스크립트 개요
grand_parent: javascript
nav_order: 2
---

# 자바스크립트 구성 요소

자바스크립트는 아래 세 가지가 합쳐진 것이다.

1. 자바스크립트 코어(ECMAscript)
2. 문서 객체 모델(DOM)
3. 브라우저 객체 모델 (BOM)

## 1. 자바스크립트 코어 (ECMAScript)

자바스크립트의 핵심(Core) 기능이다. 문법, 타입, 선언문, 키워드 등 언어의 기본 수준에 해당하는 부분이다. ECMAScript 는 자바스크립트를 이루는 코어 스크립트 언어로, 웹 환경에서만 호스트 되는 언어가 아니다. 웹 환경은 그저 ECMAScript 가 호스트되는 환경들 중 하나일 뿐이다.

## 2. 문서 객체 모델 (Document Object Model)

Document Object Model 의 표준은 W3C 에 의해 관리된다. 노드(node), 스타일, 속성, 이벤트, 위치 및 크기 등을 다룰 수 있는 다양한 기능이 포함되어 있다.

## 3. 브라우저 객체 모델 (Browser Object Model)

브라우저와 관련된 Window, Navagator, Location, History, Document, Screen 객체들이 포함되어 있다. 웹 페이지 콘텐츠와 무관하게 브라우저 기능을 노출하는 객체이다. 전역 객체인 window 의 프로퍼티와 메소드를 사용해서 제어할 수 있다. 예를 들면 BOM 으로 새 창을 연다든가, 열려있는 URL 을 파악한다든가, 브라우저의 종류를 알아낼 수 있다.

## References

* [MuckyCode 님 블로그](https://muckycode.blogspot.com/2015/01/javascript.html)
* [MDN - JavaScript 기술 개요](https://developer.mozilla.org/ko/docs/Web/JavaScript/JavaScript_technologies_overview)
