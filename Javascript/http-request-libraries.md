HTTP Request Libraries
---
# AJAX
* 비동기적 정보 교환 방식
* 자바스크립트와 xml, json 등등의 데이터 오브젝트들을 이용한 정보 교환 방식임
* 브라우저가 서버에 요청하면 서버는 순수 데이터를 응답으로 돌려주고, 브라우저는 서버에게서 받은 데이터를 잘 해석하고 가공해서 템플릿의 적절한 위치에 삽입 변경 삭제 등등을 함
* 동적 렌더링을 하기 때문에 검색 엔진에 안 걸릴 수도 있음. 근데 구글은 읽음.
* 순수 자바스크립트(XMLHttpRequest)만 사용해서 구현을 할 수도 있고, Fetch나 Axios 등의 라이브러리를 사용해서도 가능. jQuery는 $.ajax라는 함수가 있어 자체적으로 AJAX를 지원함
* 양방향 아님. 요청-응답이 끝나면 그냥 끝임. [웹소켓](https://d2.naver.com/helloworld/1336) 사용하면 양방향 가능하다는데 이 부분은 네트워크쪽에 가까운듯하여 공부가 더 필요할듯...

## Fetch API
* XMLHttpRequest의 대안
* IE 미지원.
* [요청 중간에 취소 불가능...??](https://medium.com/little-big-programming/%EB%82%B4%EA%B0%80-fetch-api%EB%A5%BC-%EC%93%B0%EC%A7%80-%EB%AA%BB%ED%96%88%EB%8D%98-%EC%9D%B4%EC%9C%A0-3c23f0ec6b82) 이슈가 close되어서 찾아보니 [여러](https://developers.google.com/web/updates/2017/09/abortable-fetch) [방법](https://github.com/whatwg/fetch/issues/447)이 있기는 한듯. 결론(?)적으로는 request.signal을 이용한 [이 방법](https://github.com/web-platform-tests/wpt/pull/6484#issuecomment-315775251)이 최선일 각????


## Axios
* Promise(ES6) 기반의 라이브러리. 설치가 필요함.
* IE 지원한다는데.. 결론적으로는 IE는 promise를 지원하지 않기 때문에 [es-promise](https://lovemewithoutall.github.io/it/vue-ie-support-with-es6-promise/)를 적용해야 함. 그런데 이거 적용하고 설치해도 안 됨. 이유는 모르겠음.
* 바벨을 사용할 경우 폴리필 쓰면 된다는데 해본적 없어서 모르겠음. 나중에 바벨을 사용하게 될 경우 테스트해봐야겠음.
* 결국 써드파티 promise library([bluebird](http://bluebirdjs.com/docs/getting-started.html)) 사용하니 되긴 하는데... 이 부분은 좀 더 생각해 봐야겠음.

## jQuery
* AJAX 메소드 주로 사용. jQuery의 `$.getJSON`, `$.get` 같은 domain-specific 메소드들이 있는데, AJAX 만큼 쉽지는 않음.


## Axios와 Fetch의 차이점
둘다 http 라이브러리이고 기능적으로 비슷.

* Fetch의 body = Axios의 data
* Fetch의 body는 stringify 적용해야 되지만, Axios의 data는 object임
* Fetch는 object를 요청할 때 url이 필요없지만, Axios는 url이 필요함
* Fetch의 request 함수는 url을 파라메터로 포함하지만, Axios의 request 함수는 포함하지 않음
* Fetch의 request는 response object가 ok 프로퍼티를 포함하고 있으면 ok라고 나오지만, Axios의 request는 status가 200이고 statusText가 OK라고 되어있어야 OK가 나옴
* json object response를 얻으려면 -> Fetch의 경우, response object에 json() 함수를 호출. Axios의 경우, response object의 data 프로퍼티를 get하면 됨.

**Fetch JSON post reuqest**
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

**Axios JSON post reuqest**
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


* https://stackoverflow.com/questions/40844297/what-is-difference-between-axios-and-fetch
* https://www.javascriptstuff.com/ajax-libraries/
* https://dzone.com/articles/top-javascript-libraries-for-making-ajax-calls