# promise

## 문제 1 : 콜백 지옥

싱글스레드인 자바스크립트에서는 비동기 처리를 위해서 콜백을 사용함. 콜백을 사용하면 비동기 처리를 잘할 수 있지만 문제는 콜백을 사용하면 비동기 처리를 중첩해서 사용하므로 예외, 에러 처리가 어렵고 복잡해짐. 이때문에 나온 단어가 콜백지옥.

참고로 비동기 함수의 결과에 대한 처리는 함수 내에서 처리해야 함. 따라서 반환 결과를 가지고 후속 처리를 할 수가 없어 코드가 중첩될 가능성이 큼. 만약 비동기 함수의 처리 결과를 가지고 다른 콜백 함수를 호출해야 하는 경우 중첩도가 높아져 콜백헬이 발생.

![callback hell](https://cdn-images-1.medium.com/max/823/1*Co0gr64Uo5kSg89ukFD2dw.jpeg)

## 문제2: 예외 처리의 한계

```text
try {
  setTimeout(() => { throw 'Error!'; }, 1000);
} catch (e) {
  console.log('에러를 캐치하지 못한다..');
  console.log(e);
}
```

여기서 catch 블록에서 에러가 캐치되지 않음. 왜 이럴까??? setTimeout 함수 내에서 에러가 발생해야 catch 구문이 이벤트 캐치를 하는데, 아이러니하게도 setTimeout 내의 콜백함수는 setTimeout 내에 존재하지 않아\(이벤트 큐에 존재\) 에러를 캐치하지 못하게 되는 것임. 왜 존재하지 않냐면 setTimeout 함수는 콜백함수의 실행을 기다리지 않고 즉시 종료되고, 정작 콜백함수는 이벤트 큐에 추가되어 호출 스택이 전부 다 비워졌을 때 비로소 실행되기 때문임.

## 해결방법: Promise

콜백지옥을 탈출하기위해 ES6에서 Promise가 등장. 쉽게 말해 성공과 실패를 분리해서 메소드를 실행하는 것. Promise 생성자 함수는 비동기 작업을 수행할 콜백 함수를 인자\(reslove와 reject\)로 전달받음.

```text
// Promise 객체의 생성
const promise = new Promise((resolve, reject) => {
  // 비동기 작업을 수행한다.

  if (/* 비동기 작업 수행 성공 */) {
    resolve('result');
  }
  else { /* 비동기 작업 수행 실패 */
    reject('failure reason');
  }
});
```

Promise는 상태 정보를 가짐. 4가지 상태가 있음.

* pending: 비동기 처리가 아직 수행되지 않은 상태 \(resolve 또는 reject 함수가 아직 호출되지 않은 상태\)
* fulfilled: 비동기 처리가 수행된 상태 \(성공\) \(resolve 함수가 호출된 상태\)
* rejected: 비동기 처리가 수행된 상태 \(실패\) \(reject 함수가 호출된 상태\)
* settled: 비동기 처리가 수행된 상태 \(성공 또는 실패\) \(resolve 또는 reject 함수가 호출된 상태\)

```text
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

프로미스의 에러 처리와 체이닝은 레퍼런스 참고하기.

## refs

[https://poiemaweb.com/es6-promise](https://poiemaweb.com/es6-promise) [https://programmingsummaries.tistory.com/325](https://programmingsummaries.tistory.com/325) [https://developers.google.com/web/fundamentals/primers/promises?hl=ko](https://developers.google.com/web/fundamentals/primers/promises?hl=ko)

