---
layout: default
title: 자바스크립트의 동작 원리
parent: 1. 자바스크립트 개요
grand_parent: javascript
nav_order: 3
---

# 자바스크립트의 동작 원리

{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 자바스크립트 엔진과 WEB API

자바스크립트 코드를 실행하는 프로그램 혹은 인터프리터라고 보면 된다. 대체로 웹브라우저에서 실행한다.

- 자바스크립트 엔진의 대표적인 예는 구글이 만든 V8 엔진이다.
- V8은 chrome과 node.js 런타임에서 사용됨.
- ECMAScript(ECMA - 262) 3rd Edition 규격의 C++로 작성되었으며, 독립적으로 실행이 가능하다.
- V8은 자바스크립트를 바이트코드(bytecode)로 컴파일하거나 인터프리트(interpret)하는 대신 실행하기 전 직접적인 기계어(x86, ARM, 또는 MIPS)로 정적 컴파일(static compile)하여 성능을 향상시켰켰다.
- 이외에도 파이어폭스에서 사용하는 스파이더몽키 등등 다른 엔진들이 많지만 동작 원리는 서로 비슷하다고 보면 된다.

### 자바스크립트 엔진의 구성 요소

자바스크립트 엔진은 다음과 같은 두 가지 주요 구성 요소로 이루어져 있다.

![자바스크립트 엔진의 구성 요소](https://t1.daumcdn.net/cfile/tistory/99747F465C3214AF30)

- 메모리 힙(Heap): 객체는 힙(구조화되지 않은 메모리 영역)에 할당된다. 변수와 객체에 대한 모든 메모리 할당은 여기서 발생한다.
- 콜 스택(Call stack): 코드가 실행될 때 콜 스택에 쌓이게 된다.

### Web API

브라우저에는 자바스크립트 개발자가 사용하는 거의 모든 API(예를 들면 setTimeout)가 있다. 그런데 이런 API들은 엔진에서 제공해주지 않는다. 그렇다면 이 API들은 어디서 오는 걸까?

사실 브라우저는 단순히 엔진 하나만으로 구성되어 있지 않고. DOM, AJAX, setTimeout 등의 브라우저에서 제공하는 Web API라고 하는 것이 있다. 이 Web API는 자바스크립트를 사용해 접근이 가능하다. Web API라는 것을 사용해 [window](https://developer.mozilla.org/en-US/docs/Web/API/Window) 객체나 [element](https://developer.mozilla.org/en-US/docs/Web/API/Element) 객체를 제어할 수도 있고, WebGL이나 Web Audio 같은 API를 사용해 복잡한 그래픽, 오디오 효과를 만들어내는 것도 가능하다. 자바스크립트 엔진이 아닌 Web API 영역에 따로 정의되어 있는 함수들은 비동기로 실행된다.

아래는 Web API의 주요 목록이다.

- [문서 객체 모델(Document Object Model)](https://developer.mozilla.org/ko/docs/DOM)
- [AJAX(XMLHttpRequest)](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest)
- [Timeout(setTimeout)](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest/timeout)

전체 목록은 [여기](https://developer.mozilla.org/en-US/docs/Web/API)서 확인 가능하다.

## 호출 스택 (콜스택, Call Stack)

자바스크립트는 단일 스레드 프로그래밍 언어이므로, 단일 호출 스택이 있다.. 단일 호출 스택이 있다는 뜻은 한 번에 하나의 일(Task)만 처리할 수 있다는 뜻이다.

콜스택의 동작 방식은 다음과 같다. **함수를 실행하면 해당 함수의 기록을 스택 맨 위에 추가(Push)**한다. **함수를 결과 값을 반환하면 스택에 쌓여있던 함수는 제거(Pop)**된다.

```js
function multiply(x, y) {
  return x * y;
}
function printSquare(x) {
  var s = multiply(x, x);
  console.log(s);
}
printSquare(5);
```

엔진이 이 코드를 실행하기 전에는 호출 스택이 비어있지만, 가장 아랫줄에 printSquare 함수가 실행되면 이후 단계는 다음과 같다.

![콜스택](https://t1.daumcdn.net/cfile/tistory/9995544C5C32151627)

## 예외 처리 시 스택의 동작

```js
function foo() {
  throw new Error("SessionStack will help you resolve crashes :)");
}
function bar() {
  foo();
}
function start() {
  bar();
}
start();
```

만약 위의 코드가 크롬에서 실행된다면 아래와 같은 순서로 에러가 발생할 것이다.

![예외 처리 시 스택의 동작](https://t1.daumcdn.net/cfile/tistory/99E900475C32153D30)

## 스택 오버플로우

이름 그대로 스택의 사이즈를 초과 했을 때 발생하는 오류이다. 스택 오버플로우는 생각보다 쉽게 일어날 수 있다. (예: 재귀를 호출했을 때.)

```js
function foo() {
  foo();
}
foo();
```

마지막 줄에서 foo() 함수가 실행되는데, foo() 함수의 내부를 살펴보면 종료 조건 없이 자신을 계속해서 호출하게 된다. 따라서 함수의 스택 프레임이 계속해서 호출 스택에 쌓이게 된다.

![스택 오버플로우](https://t1.daumcdn.net/cfile/tistory/99B00D425C3215642D)

그러다가 어떠한 시점에서 호출 스택의 함수 호출 수가 호출 스택의 실제 크기를 초과하게 되고, 브라우저는 다음과 같은 오류를 발생시키는 것으로 함수를 종료시킨다.

![스택 오버플로우 - 함수 종료](https://t1.daumcdn.net/cfile/tistory/997266485C32157D1A)

## 단일 호출 스택의 문제점

단일 스레드에서 코드를 실행하는 것은 멀티 스레드 환경에서 발생하는 복잡한 시나리오를 고려할 필요가 없으므로 매우 쉽다. 그러나 단일 스레드에서 실행하는 것도 상당히 제한적이다. 자바스크립트에서는 하나의 호출 스택만 있기 때문에, 하나의 함수 처리가 엄청 느려서 다른 함수 실행에 지장을 줄 가능성이 있다.

예를 들어, 브라우저에서 복잡한 이미지 처리를 한다고 생각해 보자. 앞서 배운 호출 스택의 동작 방식을 생각 해볼 때, 이미지 처리 작업 스택을 차지하고 있으면 자바스크립트는 후속 작업들을 처리할 수 없다. 단일 스레드, 단일 호출 스택이기 때문이다.

문제는 이것뿐만이 아니다. 브라우저가 호출 스택에서 많은 작업을 처리하기 시작하면 꽤 오랜 시간 동안 응답을 멈출 수 있다. 대부분의 브라우저는 이 상황에서 웹 페이지를 종료할지 여부를 묻는 오류 메시지를 표시한다. 그렇다면 해결 방법은 무엇일까?

![단일 호출 스택의 문제점](https://t1.daumcdn.net/cfile/tistory/99A4474F5C32193F2A)

## 비동기 콜백(Asynchronous callbacks)

가장 쉬운 해결책은 **비동기 콜백**을 사용하는 것이다. 비동기 콜백은 즉시가 아닌, 특수한 시점에 실행되므로 console.log와 같은 동기 함수와는 다르게 스택 안에 바로 push 될 필요가 없다. 그런데 이 콜백 함수들은 누가 관리하는 걸까?

## 이벤트 큐(Event Queue)와 이벤트 루프(Event Loop)

자바스크립트 실행환경(Runtime)은  Web API의 호출을 통제하기 위한 이벤트 큐와 이벤트 루프를 가지고 있다.

### 이벤트 루프

```js
while (queue.waitForMessage()) {
  queue.processNextMessage();
}
```

queue.waitForMessage()는 메시지가 처리되지 않거나 처리 대기중인 경우, 메시지가 도착할때까지 동기적으로 대기한다.

이벤트 루프는 현재 실행 중인 태스크가 없는지와 태스크 큐에 태스크가 있는지 반복적으로 확인한다. Queue에 메시지, 즉 처리해야 할 이벤트나 태스크가 존재하면
while-loop 안으로 들어가 해당하는 이벤트를 처리하거나 작업을 수행한다. 그리고는 다시 queue로 돌아와 새로운 이벤트가 존재하는지 파악하는 것이다.

## 비동기 콜백의 처리 과정

![비동기 콜백의 처리 과정](https://t1.daumcdn.net/cfile/tistory/99A7234F5C321A7F2B)

그럼 비동기가 처리되는 과정을 살펴보자.

1. 우선 버튼 클릭과 같은 이벤트가 발생하면
2. DOM 이벤트, http 요청, setTimeout 등과 같은 비동기 함수는 C++로 구현된 web API를 호출한다. 예를 들어 setTimeout의 경우, 스택에 쌓이게 되면 콜백 함수를 실행시키고 콜스택에서 사라진다.
3. web API는 콜백 함수를 이벤트 큐(콜백 큐)에 밀어넣는다. 이때 web API는 함수의 실행 순서와 상관없이 끝난 순서대로 이벤트 큐에 넣는다. setTimeout의 지연 시간이 빠른순대로 넘어간다고 생각하면 된다.
4. 그럼 이벤트 큐는 대기하다가 스택이 텅 비는 시점에 이벤트 루프를 돌리게 된다(콜스택에 넣음). 이벤트 루프의 기본 역할은 큐와 스택, 두 부분을 지켜보다가 스택이 비는 시점에 콜백을 실행시켜 주는 것.

### 이벤트 큐와 이벤트 루프의 동작을 잘 보여주는 setTimeout 예

```js
setTimeout(function() {
    console.log("first");
}, 0);
console.log("second");
```

위 코드는 second, first 순으로 출력이 된다. setTimeout에 0초의 지연시간을 주었다고 바로 실행이 되는 게 아니다. setTimeout의 콜백함수는 web API의 Timer API를 호출하고 잠시 대기하다가, 0ms 이후 이벤트 큐에 밀어넣어지게 된다. 즉, 0초 뒤에 callback 함수가 실행되는 것이 아닌, **0ms초 뒤에 callback 함수가 이벤트 큐에 들어가게 되는 것**이다.

그리고 console.log('second') 가 호출 스택에 쌓이고, second가 실행된 후 호출 스택이 비었을 때 first가 콘솔창에 나타나게 된다.

## References

- [자바스크립트 호출 스택(Call Stack) 이해하기](https://new93helloworld.tistory.com/358)
- [자바스크립트 호출 스택(Call Stack) 동작 예제](https://new93helloworld.tistory.com/361)
- [What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=1405s)
- [Web API 설명집](https://developer.mozilla.org/ko/docs/Web/%EC%B0%B8%EC%A1%B0/API)
- [Concurrency model and the event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)
