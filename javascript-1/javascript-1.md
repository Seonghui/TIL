# 1. 자바스크립트의 특징

자바스크립트는 싱글 쓰레드 기반 언어임. 호출 스택이 하나밖에는 없음. 그래서 한 번에 단 하나의 함수만 처리할 수 있음

## 자바스크립트의 동작 원리

### 자바스크립트 엔진

자바스크립트 코드를 실행하는 프로그램 혹은 인터프리터라고 보면 됨. 대체로 웹브라우저에서 실행함.

### V8

* 자바스크립트 엔진의 대표적인 예는 V8 엔진임.  구글이 만들었음.
* V8은 chrome과 node.js 런타임에서 사용됨. 
* ECMAScript\(ECMA - 262\) 3rd Edition 규격의 C++로 작성되었으며, 독립적으로 실행이 가능
* V8은 자바스크립트를 바이트코드\(bytecode\)로 컴파일하거나 인터프리트\(interpret\)하는 대신 실행하기 전 직접적인 기계어\(x86, ARM, 또는 MIPS\)로 정적 컴파일\(static compile\)하여 성능을 향상시켰음
* 이외에도 파이어폭스에서 사용하는 스파이더몽키 등등 다른 엔진들이 많지만 동작 원리는 서로 비슷하다고 보면 됨.

## 자바스크립트 구성 요소

자바스크립트는 아래 세 가지가 합쳐진 것이다.

1. 자바스크립트 코어\(ECMAscript\)
2. 문서 객체 모델\(DOM\)
3. 브라우저 객체 모델 \(BOM\)

### 1. 자바스크립트 코어 \(ECMAScript\)

자바스크립트의 핵심\(Core\) 기능이다. 문법, 타입, 선언문, 키워드 등 언어의 기본 수준에 해당하는 부분이다. ECMAScript 는 자바스크립트를 이루는 코어 스크립트 언어로, 웹 환경에서만 호스트 되는 언어가 아니다. 웹 환경은 그저 ECMAScript 가 호스트되는 환경들 중 하나일 뿐이다.

### 2. 문서 객체 모델 \(Document Object Model\)

Document Object Model 의 표준은 W3C 에 의해 관리된다. 노드\(node\), 스타일, 속성, 이벤트, 위치 및 크기 등을 다룰 수 있는 다양한 기능이 포함되어 있다.

### 3. 브라우저 객체 모델 \(Browser Object Model\)

브라우저와 관련된 Window, Navagator, Location, History, Document, Screen 객체들이 포함되어 있다. 웹 페이지 콘텐츠와 무관하게 브라우저 기능을 노출하는 객체이다. 전역 객체인 window 의 프로퍼티와 메소드를 사용해서 제어할 수 있다. 예를 들면 BOM 으로 새 창을 연다든가, 열려있는 URL 을 파악한다든가, 브라우저의 종류를 알아낼 수 있다.

## References

* [MuckyCode 님 블로그](https://muckycode.blogspot.com/2015/01/javascript.html)
* MDN \([https://developer.mozilla.org/ko/docs/Web/JavaScript/JavaScript\_technologies\_overview](https://developer.mozilla.org/ko/docs/Web/JavaScript/JavaScript_technologies_overview)\)

