# 자바스크립트의 동작 원리

## 자바스크립트 엔진

자바스크립트 코드를 실행하는 프로그램 혹은 인터프리터라고 보면 된다. 대체로 웹브라우저에서 실행한다.

* 자바스크립트 엔진의 대표적인 예는 구글이 만든 V8 엔진이다.
* V8은 chrome과 node.js 런타임에서 사용됨.
* ECMAScript\(ECMA - 262\) 3rd Edition 규격의 C++로 작성되었으며, 독립적으로 실행이 가능하다.
* V8은 자바스크립트를 바이트코드\(bytecode\)로 컴파일하거나 인터프리트\(interpret\)하는 대신 실행하기 전 직접적인 기계어\(x86, ARM, 또는 MIPS\)로 정적 컴파일\(static compile\)하여 성능을 향상시켰켰다.
* 이외에도 파이어폭스에서 사용하는 스파이더몽키 등등 다른 엔진들이 많지만 동작 원리는 서로 비슷하다고 보면 된다.

### 자바스크립트 엔진의 구성 요소

자바스크립트 엔진은 다음과 같은 두 가지 주요 구성 요소로 이루어져 있다.

![자바스크립트 엔진의 구성 요소](https://t1.daumcdn.net/cfile/tistory/99747F465C3214AF30)

* 메모리 힙: 객체는 힙\(구조화되지 않은 메모리 영역\)에 할당됨. 변수와 객체에 대한 모든 메모리 할당은 여기서 발생
* 콜 스택: 코드가 실행될 때 콜 스택이 쌓임.\(실행 컨택스트\)

## 실행 환경 (Runtime)

브라우저에는 자바스크립트 개발자가 사용하는 거의 모든 API(예를 들면 setTimeout)가 있다. 그런데 이런 API들은 엔진에서 제공해주지 않는다. 그렇다면 이 API들은 어디서 오는 걸까?

![실행 환경](https://t1.daumcdn.net/cfile/tistory/999DB3485C3214E122)

사실 브라우저는 단순히 엔진 하나만으로 구성되어 있지 않다. DOM, AJAX, setTimeout 등의 브라우저에서 제공하는 Web API라고 하는 것이 있다. 그리고 이러한 Web API의 호출을 통제하기 위한 Event Queue와 Event Loop도 존재한다.

## 호출 스택 (Call Stack)

자바스크립트는 단일 스레드 프로그래밍 언어이므로, 단일 호출 스택이 있습니다. 단일 호출 스택이 있다는 뜻은 한 번에 하나의 일(Task)만 처리할 수 있다는 뜻입니다.

호출 스택이란 프로그램에서 우리가 어디에 있는지를 기본적으로 기록하는 데이터 구조입니다. 동작 방식은 다음과 같습니다. 함수를 실행하면 해당 함수의 기록을 스택 맨 위에 추가(Push) 합니다. 우리가 함수를 결과 값을 반환하면 스택에 쌓여있던 함수는 제거(Pop) 됩니다. 예제를 살펴보도록 하죠.

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

엔진이 이 코드를 실행하기 전에는 호출 스택이 비어있습니다. 가장 아랫줄에 printSquare 함수가 실행되면 이후 단계는 다음과 같습니다

![콜스택](https://t1.daumcdn.net/cfile/tistory/9995544C5C32151627)

## 예외 처리 시 스택의 동작

아래의 코드는 예외가 던져질 때 스택의 호출이 어떤 순서로 일어나는지 알려줍니다. 다음의 코드를 살펴봅시다.

```js
function foo() {
    throw new Error('SessionStack will help you resolve crashes :)');
}
function bar() {
    foo();
}
function start() {
    bar();
}
start();
```

만약 위의 코드가 크롬에서 실행된다면 아래와 같은 순서로 에러가 발생할 것입니다.

![예외 처리 시 스택의 동작](https://t1.daumcdn.net/cfile/tistory/99E900475C32153D30)

## 스택 오버플로우

이름 그대로 스택의 사이즈를 초과 했을 때 발생하는 오류입니다. 스택 오버플로우는 생각보다 쉽게 일어날 수 있습니다. 특히 재귀를 호출했을 때 말이죠.

```js
function foo() {
    foo();
}
foo();
```

마지막 줄에서 foo() 함수가 실행되는데, foo() 함수의 내부를 살펴보면 종료 조건 없이 자신을 계속해서 호출하게 됩니다. 따라서 함수의 스택 프레임이 계속해서 호출 스택에 쌓이게 됩니다.

![스택 오버플로우](https://t1.daumcdn.net/cfile/tistory/99B00D425C3215642D)

그러다가 어떠한 시점에서 호출 스택의 함수 호출 수가 호출 스택의 실제 크기를 초과하게 되고, 브라우저는 다음과 같은 오류를 발생시키는 것으로 함수를 종료 시킵니다.

![스택 오버플로우 - 함수 종료](https://t1.daumcdn.net/cfile/tistory/997266485C32157D1A)

## 단일 호출 스택의 문제점

단일 스레드에서 코드를 실행하는 것은 멀티 스레드 환경에서 발생하는 복잡한 시나리오(예: deadlocks)를 고려할 필요가 없으므로 매우 쉽습니다. 그러나 단일 스레드에서 실행하는 것도 상당히 제한적입니다. 자바스크립트에서는 하나의 호출 스택만 있기 때문에, 하나의 함수 처리가 엄청 느려서 다른 함수 실행에 지장을 줄 때는 어떻게 해야 할까요?

예를 들어, 브라우저에서 복잡한 이미지 처리를 한다고 생각해봅시다. 앞서 배운 호출 스택의 동작 방식을 생각 해볼 때, 이미지 처리 작업 스택을 차지하고 있으면 자바스크립트는 후속 작업들을 처리할 수 없습니다. 단일 스레드, 단일 호출 스택이기 때문입니다.

문제는 이것뿐만이 아닙니다. 브라우저가 호출 스택에서 많은 작업을 처리하기 시작하면 꽤 오랜 시간 동안 응답을 멈출 수 있습니다. 대부분의 브라우저는 이 상황에서 웹 페이지를 종료할지 여부를 묻는 오류 메시지를 표시합니다. 그렇다면 해결 방법은 무엇일까요?

![단일 호출 스택의 문제점](https://t1.daumcdn.net/cfile/tistory/99A4474F5C32193F2A)

## 비동기 콜백(Asynchronous callbacks)

가장 쉬운 해결책은 비동기 콜백을 사용하는 것입니다. 즉, 우리의 코드 일부를 실행하고 나중에 실행될 콜백(함수)를 제공합니다. 비동기 콜백은 즉시가 아닌, 특수한 시점에 실행되므로 console.log와 같은 동기 함수와는 다르게 스택 안에 바로 push 될 필요가 없습니다. 그런데 스택이 아니라면 이 콜백 함수들은 누가 관리하는 걸까요?

## 이벤트 큐(Event Queue)와 비동기 콜백의 처리 과정

자바스크립트 실행환경(Runtime)은 이벤트 큐(Event Queue)를 가지고 있습니다. 이는 처리할 메시지 목록과 실행할 콜백 함수 들의 리스트입니다.

![이벤트 큐](https://t1.daumcdn.net/cfile/tistory/99A7234F5C321A7F2B)

그럼 비동기가 처리되는 과정을 살펴봅시다. 우선 버튼 클릭과 같은 이벤트가 발생하면 DOM 이벤트, http 요청, setTimeout 등과 같은 비동기 함수는 C++로 구현된 web API를 호출하며, web API는 콜백 함수를 이벤트 큐(콜백 큐)에 밀어 넣습니다. (이때 web API는 함수의 실행 순서와 상관없이 끝난 순서대로 이벤트 큐에 넣습니다. setTimeout의 지연 시간이 빠른순대로 넘어간다고 생각하면 됨...) 그럼 이벤트 큐는 대기하다가 스택이 텅 비는 시점에 이벤트 루프를 돌리게 됩니다(스택에 넣음). 이벤트 루프의 기본 역할은 큐와 스택, 두 부분을 지켜보다가 스택이 비는 시점에 콜백을 실행시켜 주는 것. 각 메시지와 콜백은 다른 메시지가 처리되기 전에 완전히 처리됩니다.

웹 브라우저에서는 이벤트가 발생할 때마다 메시지가 추가되고 이벤트 리스너가 첨부됩니다. 따라서 리스너가 없으면 이벤트가 손실됩니다. 콜백 함수의 호출은 호출 스택의 초기 프레임으로 사용되며, 자바스크립트가 싱글 스레드이므로 스택에 대한 모든 호출이 반환될 때까지 메세지 폴링(polling) 및 처리가 중지됩니다. 동기식 함수 호출은 이와 반대로 새 호출 프레임을 스택에 추가합니다.

## refs

* [자바스크립트 호출 스택(Call Stack) 이해하기](https://new93helloworld.tistory.com/358)
* [자바스크립트 호출 스택(Call Stack) 동작 예제](https://new93helloworld.tistory.com/361)
* [What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ&t=1405s)