# 단위 테스트

## 규칙 정하기

* 독립적이여야 한다. 어떤 테스트도 다른 테스트에 의존하지 않아야 한다. 한 번에 한 가지만 테스트하기.
* 격리되어야 한다. ajax, localstorage 등 테스트 대상이 의존하는 것을 다른 것으로 대체한다. \(...어떻게?\)
* given, when, then 단계에 따라 테스트 코드를 작성한다.
* 간단한 것부터 먼저 테스트한다.
* 테스트의 이름은 명확하게 써야함. 길어도 됨.

## 스토리 짜기

시나리오를 작성해야 테스트를 진행할 수 있다. \(ux 페르소나 설정하는 것과 비슷\)

## 테스트 사이클

시나리오 작성 -&gt; 테스트 코드 작성 -&gt; 기능 구현 -&gt; 리팩토링 무한반복

## refs

* [https://medium.com/@jinseok.choi/jest%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-unit-test-%EC%A0%81%EC%9A%A9%EA%B8%B0-420049c16cc8](https://medium.com/@jinseok.choi/jest%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-unit-test-%EC%A0%81%EC%9A%A9%EA%B8%B0-420049c16cc8)
* [https://riptutorial.com/ko/unit-testing/topic/9947/%EB%AA%A8%EB%93%A0-%EC%96%B8%EC%96%B4%EC%97%90-%EB%8C%80%ED%95%9C-%EB%8B%A8%EC%9C%84-%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%A5%BC%EC%9C%84%ED%95%9C-%EC%9D%BC%EB%B0%98%EC%A0%81%EC%9D%B8-%EA%B7%9C%EC%B9%99](https://riptutorial.com/ko/unit-testing/topic/9947/%EB%AA%A8%EB%93%A0-%EC%96%B8%EC%96%B4%EC%97%90-%EB%8C%80%ED%95%9C-%EB%8B%A8%EC%9C%84-%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%A5%BC%EC%9C%84%ED%95%9C-%EC%9D%BC%EB%B0%98%EC%A0%81%EC%9D%B8-%EA%B7%9C%EC%B9%99)

