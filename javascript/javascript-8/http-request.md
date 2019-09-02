# HTTP Request

## 비동기(Asynchronous)와 동기(synchronous)의 개념

### 동기(synchronous : 동시에 일어나는)

동기는 말 그대로 동시에 일어난다는 뜻입니다. 요청과 그 결과가 동시에 일어난다는 약속인데요. 바로 요청을 하면 시간이 얼마가 걸리던지 요청한 자리에서 결과가 주어져야 합니다.

### 비동기(Asynchronous : 동시에 일어나지 않는)

비동기는 동시에 일어나지 않는다를 의미합니다. 요청과 결과가 동시에 일어나지 않을거라는 약속입니다.

### 동기 방식과 비동기 방식의 장단점

동기와 비동기는 상황에 따라서 각각의 **장단점**이 있습니다.

**동기방식**은 설계가 매우 간단하고 직관적이지만 결과가 주어질 때까지 아무것도 못하고 대기해야 하는 단점이 있고,

**비동기방식**은 동기보다 복잡하지만 결과가 주어지는데 시간이 걸리더라도 그 시간 동안 다른 작업을 할 수 있으므로 자원을 효율적으로 사용할 수 있는 장점이 있습니다.

동기 함수는 블로킹인 반면, 비동기 함수는 그렇지 않습니다. 동기 함수에서는 다음 명령문이 실행되기 전에 앞 명령문이 완료됩니다. 이 경우, 프로그램은 명령문의 순서대로 정확하게 평가되고 명령문 중 하나가 매우 오랜 시간이 걸리면 프로그램 실행이 일시중지됩니다.

비동기 함수는 일반적으로 파라미터를 통해서 콜백을 받고, 비동기 함수가 호출된 후 즉시 다음 줄 실행이 계속됩니다. 콜백은 비동기 작업이 완료되고 호출 스택이 비어 있을 때만 호출됩니다. 웹 서버에서 데이터를 로드하거나 데이터베이스를 쿼리하는 등의 무거운 작업을 비동기식으로 수행하여, 메인 스레드가 긴 작업을 완료할 때까지 블로킹하지 않고 다른 작업을 계속할 수 있습니다(브라우저의 경우 UI가 중지됨).

## 1. AJAX

* 비동기적 정보 교환 방식
* 자바스크립트와 xml, json 등등의 데이터 오브젝트들을 이용한 정보 교환 방식임
* 브라우저가 서버에 요청하면 서버는 순수 데이터를 응답으로 돌려주고, 브라우저는 서버에게서 받은 데이터를 잘 해석하고 가공해서 템플릿의 적절한 위치에 삽입 변경 삭제 등등을 함
* 동적 렌더링을 하기 때문에 검색 엔진에 안 걸릴 수도 있음. 근데 구글은 읽음.
* 순수 자바스크립트\(XMLHttpRequest\)만 사용해서 구현을 할 수도 있고, Fetch나 Axios 등의 라이브러리를 사용해서도 가능. jQuery는 $.ajax라는 함수가 있어 자체적으로 AJAX를 지원함
* 양방향 아님. 요청-응답이 끝나면 그냥 끝임. [웹소켓](https://d2.naver.com/helloworld/1336) 사용하면 양방향 가능하다는데 이 부분은 네트워크쪽에 가까운듯하여 공부가 더 필요할듯...

## 2. Fetch API

```javascript
fetch('example.json', {
  method: "GET",
  headers: {
    "Content-Type": "application/json"
  }
}).then(function(res) { // 헤더 부근까지 읽기를 마친 시점에서 호출
  if (res.ok) {
    alert('succeed');
  } else if (res.status == 401) {
    alert('401 error');
  }
}, function(e) {
  alert('error');
});
```

* XMLHttpRequest의 대안.
* XMLHttpRequest보다 오리진 서버 밖으로의 액세스 등 CORS 제어가 쉬워진다.
* 자바스크립트의 모던한 비동기 처리 작성 기법인 프로미스를 따른다.
* 캐시, 리퍼러 정책을 설정할 수 있다. 캐시 제어는 Fetch API에서만 할 수 있다.
* IE 미지원.

## 3. Promise

### 3-1. 문제 1 - 콜백 지옥

싱글스레드인 자바스크립트에서는 비동기 처리를 위해서 콜백을 사용함. 콜백을 사용하면 비동기 처리를 잘할 수 있지만 문제는 콜백을 사용하면 비동기 처리를 중첩해서 사용하므로 예외, 에러 처리가 어렵고 복잡해짐. 이때문에 나온 단어가 콜백지옥.

참고로 비동기 함수의 결과에 대한 처리는 함수 내에서 처리해야 함. 따라서 반환 결과를 가지고 후속 처리를 할 수가 없어 코드가 중첩될 가능성이 큼. 만약 비동기 함수의 처리 결과를 가지고 다른 콜백 함수를 호출해야 하는 경우 중첩도가 높아져 콜백헬이 발생.

![callback hell](https://cdn-images-1.medium.com/max/823/1*Co0gr64Uo5kSg89ukFD2dw.jpeg)

### 3-2. 문제2 - 예외 처리의 한계

```js
try {
  setTimeout(() => { throw 'Error!'; }, 1000);
} catch (e) {
  console.log('에러를 캐치하지 못한다..');
  console.log(e);
}
```

여기서 catch 블록에서 에러가 캐치되지 않음. 왜 이럴까??? setTimeout 함수 내에서 에러가 발생해야 catch 구문이 이벤트 캐치를 하는데, 아이러니하게도 setTimeout 내의 콜백함수는 setTimeout 내에 존재하지 않아\(이벤트 큐에 존재\) 에러를 캐치하지 못하게 되는 것임. 왜 존재하지 않냐면 setTimeout 함수는 콜백함수의 실행을 기다리지 않고 즉시 종료되고, 정작 콜백함수는 이벤트 큐에 추가되어 호출 스택이 전부 다 비워졌을 때 비로소 실행되기 때문임.

### 3-3. 해결방법 - Promise

콜백지옥을 탈출하기위해 ES6에서 Promise가 등장. 쉽게 말해 성공과 실패를 분리해서 메소드를 실행하는 것. Promise 생성자 함수는 비동기 작업을 수행할 콜백 함수를 인자\(reslove와 reject\)로 전달받음. Promise는 then 함수로 꼬리에 꼬리를 잇는 체이닝 방식으로 비동기 방식을 순차적으로 처리할 수 있다.

```js
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

#### 프로미스 체이닝 예시

```js
new Promise(function(resolve, reject) {

  setTimeout(() => resolve(1), 1000); // (*)

}).then(function(result) { // (**)

  alert(result); // 1
  return result * 2;

}).then(function(result) { // (***)

  alert(result); // 2
  return result * 2;

}).then(function(result) {

  alert(result); // 4
  return result * 2;

});
```

## 4. Axios

* Promise\(ES6\) 기반의 라이브러리. 설치가 필요함.
* IE 지원한다는데.. 결론적으로는 IE는 promise를 지원하지 않기 때문에 [es-promise](https://lovemewithoutall.github.io/it/vue-ie-support-with-es6-promise/)를 적용해야 함. 그런데 이거 적용하고 설치해도 안 됨. 이유는 모르겠음.
* 바벨을 사용할 경우 폴리필 쓰면 된다는데 해본적 없어서 모르겠음. 나중에 바벨을 사용하게 될 경우 테스트해봐야겠음.
* 결국 써드파티 promise library\([bluebird](http://bluebirdjs.com/docs/getting-started.html)\) 사용하니 되긴 하는데... 이 부분은 좀 더 생각해 봐야겠음.

## 5. jQuery

* AJAX 메소드 주로 사용. jQuery의 `$.getJSON`, `$.get` 같은 domain-specific 메소드들이 있는데, AJAX 만큼 쉽지는 않음.

## 6. Axios와 Fetch의 차이점

둘다 http 라이브러리이고 기능적으로 비슷.

* Fetch의 body = Axios의 data
* Fetch의 body는 stringify 적용해야 되지만, Axios의 data는 object임
* Fetch는 object를 요청할 때 url이 필요없지만, Axios는 url이 필요함
* Fetch의 request 함수는 url을 파라메터로 포함하지만, Axios의 request 함수는 포함하지 않음
* Fetch의 request는 response object가 ok 프로퍼티를 포함하고 있으면 ok라고 나오지만, Axios의 request는 status가 200이고 statusText가 OK라고 되어있어야 OK가 나옴
* json object response를 얻으려면 -&gt; Fetch의 경우, response object에 json\(\) 함수를 호출. Axios의 경우, response object의 data 프로퍼티를 get하면 됨.

### Fetch JSON post reuqest

```javascript
let url = 'https://someurl.com';
let options = {
            method: 'POST',
            mode: 'cors',
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json;charset=UTF-8'
            },
            body: JSON.stringify({
                property_one: value_one,
                property_two: value_two
            })
        };
let response = await fetch(url, options);
let responseOK = response && response.ok;
if (responseOK) {
    let data = await response.json();
    // do something with data
}
```

### Axios JSON post reuqest

```javascript
let url = 'https://someurl.com';
let options = {
            method: 'POST',
            url: url,
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json;charset=UTF-8'
            },
            data: {
                property_one: value_one,
                property_two: value_two
            }
        };
let response = await axios(options);
let responseOK = response && response.status === 200 && response.statusText === 'OK';
if (responseOK) {
    let data = await response.data;
    // do something with data
}
```

## 7. async and await

우선 함수 앞에 async를 붙여주고 비동기로 처리되는 곳에 await를 추가합니다. 여기서 주의할 점은 await 뒷부분은 반드시 promise를 반환해야하며, async함수 자체도 promise를 반환합니다. async await은 거의 동기 코드랑 비슷하게 짤 수 있게 해줍니다.

```js
function doubleAfter2Seconds(x) {
  console.log('value:' + x)
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x * 2);
    }, 2000);
  });
}
async function addAsync(x) {
  const a = await doubleAfter2Seconds(10);
  const b = await doubleAfter2Seconds(20);
  const c = await doubleAfter2Seconds(30);
  return x + a + b + c;
}
addAsync(10).then((sum) => {
  console.log('result:' + sum);
});
```

async 함수가 호출되면 Promise를 리턴합니다. async함수에서 값을 리턴하면, promise는 그 값을 받아서 resolved됩니다. async함수는 await 표현식을 포함하고 있으며 async 함수에 Promise 값이 전달되기 전까지 실행을 지연시킵니다.

## References

* 리얼월드 HTTP
* [What is difference between Axios and Fetch?](https://stackoverflow.com/questions/40844297/what-is-difference-between-axios-and-fetch)
* [AJAX/HTTP Library Comparison](https://www.javascriptstuff.com/ajax-libraries/)
* [Top JavaScript Libraries for Making AJAX Calls](https://dzone.com/articles/top-javascript-libraries-for-making-ajax-calls)
* [동기와 비동기의 개념과 차이](https://private.tistory.com/24)
* [자바스크립트 비동기 프로그래밍 promise, async, await](https://medium.com/@shlee1353/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%B9%84%EB%8F%99%EA%B8%B0-async-await-promise-ae659eb1cb7e)
* [[JS] async/await으로 콜백지옥을 해결해보자](https://victorydntmd.tistory.com/87)
* [poiemaweb - primise](https://poiemaweb.com/es6-promise)
* [바보들을 위한 Promise 강의](https://programmingsummaries.tistory.com/325)
* [자바스크립트 프라미스: 소개](https://developers.google.com/web/fundamentals/primers/promises?hl=ko)
* [Youtube - 비동기 프로그래밍이 뭔가요?](https://www.youtube.com/watch?v=m0icCqHY39U)
* [Promises chaining](https://javascript.info/promise-chaining)
