---
layout: default
title: Promise
parent: 8. 비동기적 프로그래밍
grand_parent: javascript
nav_order: 2
---

## 1. Promise

### 1-1. 문제1 - 콜백 지옥

싱글스레드인 자바스크립트에서는 비동기 처리를 위해서 콜백을 사용한다. 콜백을 사용하면 비동기 처리를 잘할 수 있지만 문제는 콜백을 사용하면 비동기 처리를 중첩해서 사용하므로 예외, 에러 처리가 어렵고 복잡해진다. 이때문에 나온 단어가 바로 콜백지옥이다.

참고로 비동기 함수의 결과에 대한 처리는 함수 내에서 처리해야 한다. 따라서 반환 결과를 가지고 후속 처리를 할 수가 없어 코드가 중첩될 가능성이 크다. 만약 비동기 함수의 처리 결과를 가지고 다른 콜백 함수를 호출해야 하는 경우 중첩도가 높아져 콜백지옥이 발생하게 되는 것이다.

![callback hell](https://cdn-images-1.medium.com/max/823/1*Co0gr64Uo5kSg89ukFD2dw.jpeg)

### 1-2. 문제2 - 예외 처리의 한계

```js
try {
  setTimeout(() => { throw 'Error!'; }, 1000);
} catch (e) {
  console.log('에러를 캐치하지 못한다..');
  console.log(e);
}
```

여기서 catch 블록에서 에러가 캐치되지 않는다. 왜 그럴까?

setTimeout 함수 내에서 에러가 발생해야 catch 구문이 이벤트 캐치를 하는데, 아이러니하게도 **setTimeout 내의 콜백함수는 setTimeout 내에 존재하지 않고 이벤트 큐에 존재**해서 에러를 캐치하지 못하게 되는 것이다. 왜 존재하지 않냐면, setTimeout 함수는 콜백함수의 실행을 기다리지 않고 즉시 종료되고, 정작 콜백함수는 이벤트 큐에 추가되어 호출 스택이 전부 다 비워졌을 때 비로소 실행되기 때문이다.

### 1-3. 해결방법 - Promise

콜백지옥을 탈출하기위해 ES6에서 Promise가 등장했다. 프로미스의 목적은 **성공과 실패를 분리해서 메소드를 실행**하는 것이다. Promise 생성자 함수는 비동기 작업을 수행할 콜백 함수를 인자\(reslove와 reject\)로 전달받는다. Promise는 then 함수로 꼬리에 꼬리를 잇는 체이닝 방식으로 비동기 방식을 순차적으로 처리할 수 있다.

resolve를 통해서 결과값을 넘기게 되면, 그 결과값이 then에서 받은 함수에게 넘겨지게 된다. promise를 사용하기 전에는 콜백 안에 콜백을 넣는 식으로 사용했다면, 이제는 then을 이어서 (프로미스 체이닝) 콜백 없이도 구현할 수 있게 해준다. 이때 다음 then의 실행조건은 이전 then에서 실행한 promise가 resolve 되었을 때이다.

##### 구문

```js
new Promise(executor)
```

`executor` 파라메터는 `resolve`와 `reject`를 인수로 전달하는 함수이다. 이 함수는 즉시 실행된다. 비동기 작업이 성공하면 `resolve`을 호출해 프로미스를 이행하고, 오류가 발생하면 `reject` 함수를 호출해 거부하게 된다.



다음은 Promise 객체를 생성하는 방법이다.

```js
// Promise 객체의 생성
const promise = new Promise((resolve, reject) => {
  // 비동기 작업을 수행한다.

  if (/* 비동기 작업 수행 성공 */) {
    resolve('success');
  }
  else { /* 비동기 작업 수행 실패 */
    reject('failure');
  }
});
```

다음은 해당 Promise를 사용하는 방법이다.

```js
promise.then(function(result) {
  console.log(result); // success
}, function(error) {
  console.log(error); // failure
})
```

`then()`은 성공 사례에 대한 콜백과 실패 사례에 대한 콜백이라는 두 인수를 취한다. 아래와 같은 모양새이다.

```js
promise.then(성공 콜백, 실패 콜백)
```

둘 다 선택 사항이라, 성공 콜백이나 실패 콜백 중 하나만 추가할 수도 있다.

다음과 같이 catch를 사용할 수도 있다.

```js
promise.then(function(result) {
  console.log(result); // success
}).catch(function(error) {
  console.log(error); // failure
})
```

#### 1-3-1. Promise의 상태

Promise는 4가지 상태를 가진다.

* pending: 비동기 처리가 아직 수행되지 않은 상태 \(resolve 또는 reject 함수가 아직 호출되지 않은 상태\)
* fulfilled: 비동기 처리가 수행된 상태 \(성공\) \(resolve 함수가 호출된 상태\)
* rejected: 비동기 처리가 수행된 상태 \(실패\) \(reject 함수가 호출된 상태\)
* settled: 비동기 처리가 수행된 상태 \(성공 또는 실패\) \(resolve 또는 reject 함수가 호출된 상태\)

```html
<!DOCTYPE html>
<html>
<head>
  <title>Promise example</title>
</head>
<body>
  <h1>Promise example</h1>
  <pre id="result"></pre>
  <script>
  // 비동기 함수
  function get(url) {
    // Promise 객체의 생성과 반환
    return new Promise((resolve, reject) => {
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // 서버 응답 시 호출될 이벤트 핸들러
      xhr.onreadystatechange = function () {
        // 서버 응답 완료
        if (xhr.readyState === XMLHttpRequest.DONE) {
          if (xhr.status === 200) { // 정상 응답
            // resolve 메소드에 처리 결과를 전달
            resolve(xhr.response);
          } else { // 비정상 응답
            // reject 메소드에 에러 메시지를 전달
            reject('Error: ' + xhr.status);
          }
        }
      };

      // 비동기 방식으로 Request를 오픈한다
      xhr.open('GET', url);
      // Request를 전송한다
      xhr.send();
    });
  }

  const url = 'http://jsonplaceholder.typicode.com/post/1';

  /*
    비동기 함수 get은 Promise 객체를 반환한다.
    Promise 객체의 후속 메소드를 사용하여 비동기 처리 결과에 대한 후속 처리를 수행한다.
  */
  get(url).then(
    // 첫 번째 콜백 함수는 성공(fulfilled, resolve 함수가 호출된 상태) 시 호출된다.
    result => document.getElementById('result').innerHTML = result,
    // 두 번째 함수는 실패(rejected, reject 함수가 호출된 상태) 시 호출된다.
    error => console.log(error)
  );
  </script>
</body>
</html>
```

#### 1-3-2. 프로미스 체이닝 예시

결과가 `.then` 핸들러의 체인을 통해 전달된다. 이렇게 프로미스의 체이닝이 가능한 이유는 promise.then을 호출하면 **프로미스가 반환되기 때문**이다.

```js
var promise = new Promise(function(resolve, reject) {
  resolve(1);
});

promise.then(function(val) {
  console.log(val); // 1
  return val + 2;
}).then(function(val) {
  console.log(val); // 3
})
```

#### 1-3-3. Promise.all

Promise.all은 여러 개의 프로미스가 모두 완료되었을 때 실행하는 방법이다.

##### 구문

```js
Promise.all(iterable);
```

iterable은 Array와 같이 순회 가능한(iterable) 객체이다.

```js
const param = true;

const promise1 = new Promise((resolve, reject) => {
  if (param) {
    resolve('success');
  }
  else {
    reject('failure');
  }
});

const promise2 = new Promise((resolve, reject) => {
  if (param) {
    resolve('success');
  } else {
    reject('failure');
  }
})

Promise.all([promise1, promise2]).then((values) => {
  console.log('모두 완료', values); // '모두 완료', ['promise1', 'promise2']
})
```

## References

* [Promises chaining](https://javascript.info/promise-chaining)
* [poiemaweb - promise](https://poiemaweb.com/es6-promise)
* [바보들을 위한 Promise 강의](https://programmingsummaries.tistory.com/325)
* [자바스크립트 프라미스: 소개](https://developers.google.com/web/fundamentals/primers/promises?hl=ko)