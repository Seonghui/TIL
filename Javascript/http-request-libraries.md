HTTP Request Libraries
---

## Fetch API
* XMLHttpRequest의 대안
* IE 미지원.
* [요청 중간에 취소 불가능...??](https://medium.com/little-big-programming/%EB%82%B4%EA%B0%80-fetch-api%EB%A5%BC-%EC%93%B0%EC%A7%80-%EB%AA%BB%ED%96%88%EB%8D%98-%EC%9D%B4%EC%9C%A0-3c23f0ec6b82) 이슈가 close되어서 찾아보니 [여러](https://developers.google.com/web/updates/2017/09/abortable-fetch) [방법](https://github.com/whatwg/fetch/issues/447)이 있기는 한듯. 결론(?)적으로는 request.signal을 이용한 [이 방법](https://github.com/web-platform-tests/wpt/pull/6484#issuecomment-315775251)이 최선일 각????


## Axios
* Promise(ES6) 기반의 라이브러리. 설치가 필요함.


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


https://stackoverflow.com/questions/40844297/what-is-difference-between-axios-and-fetch
https://www.javascriptstuff.com/ajax-libraries/
https://dzone.com/articles/top-javascript-libraries-for-making-ajax-calls