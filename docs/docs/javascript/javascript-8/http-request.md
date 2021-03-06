---
layout: default
title: HTTP Request
parent: 8. 비동기적 프로그래밍
grand_parent: javascript
nav_order: 1
---

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

## 3. Axios

* Promise\(ES6\) 기반의 라이브러리. 설치가 필요함.
* IE 지원한다는데.. 결론적으로는 IE는 promise를 지원하지 않기 때문에 [es-promise](https://lovemewithoutall.github.io/it/vue-ie-support-with-es6-promise/)를 적용해야 함. 그런데 이거 적용하고 설치해도 안 됨. 이유는 모르겠음.
* 바벨을 사용할 경우 폴리필 쓰면 된다는데 해본적 없어서 모르겠음. 나중에 바벨을 사용하게 될 경우 테스트해봐야겠음.
* 결국 써드파티 promise library\([bluebird](http://bluebirdjs.com/docs/getting-started.html)\) 사용하니 되긴 하는데... 이 부분은 좀 더 생각해 봐야겠음.

## 4. jQuery

* AJAX 메소드 주로 사용. jQuery의 `$.getJSON`, `$.get` 같은 domain-specific 메소드들이 있는데, AJAX 만큼 쉽지는 않음.

## 5. Axios와 Fetch의 차이점

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

## References

* 리얼월드 HTTP
* [What is difference between Axios and Fetch?](https://stackoverflow.com/questions/40844297/what-is-difference-between-axios-and-fetch)
* [AJAX/HTTP Library Comparison](https://www.javascriptstuff.com/ajax-libraries/)
* [Top JavaScript Libraries for Making AJAX Calls](https://dzone.com/articles/top-javascript-libraries-for-making-ajax-calls)
* [동기와 비동기의 개념과 차이](https://private.tistory.com/24)
* [Youtube - 비동기 프로그래밍이 뭔가요?](https://www.youtube.com/watch?v=m0icCqHY39U)
