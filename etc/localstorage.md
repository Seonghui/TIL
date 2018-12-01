로컬 스토리지(Local Storage)
===

# 웹스토리지(Web Storage)
웹스토리지는 기존 쿠키의 한계성 때문에 HTML5의 새로운 기능으로 추가된 기능임. 웹스토리지는 크게 두 가지로 나뉘어짐.

* 로컬 스토리지(Local Storage): 강제로 지우지 않는 이상 클라이언트에 대한 정보를 영구적으로 보관함 로컬 스토리지의 경우 기본적으로 5mb라는 용량 제한이 있음. (참고로 쿠키는 4KB)
* 세션 스토리지(Session Storage): 현재 세션 동안, 즉 브라우저나 탭을 닫지 않을 경우 유지됨. 용량 제한이 없음.

## 웹 스토리지의 용도
서버에 저장하는 경우 데이터의 동기화가 필요하기 때문에 최소화하는 것이 좋음. 데이터 크기와 만료 시간을 고려해 서버에 저장하기 애매한 데이터들을 저장하는 것이 좋음. 단 보안 이슈가 있어서 암호화가 필요한 중요한 정보들은 저장하지 말아야 함. 웹스토리지를 암호화하는 방법이 있기는 한데 굳이?라는 생각이 든다.

* 기존 쿠키로 구현한 기능 대체 가능
* 글 임시저장 기능
* 장바구니 기능 등등

# 로컬 스토리지
로컬 스토리지는 웹스토리지에서 지원하는 스토리지 방식 중 하나임. 브라우저간 스토리지 공유는 되지 않고 쿠키보다 용량이 크다는 단점이 있다. 세션스토리지도 로컬 스토리지랑 비슷하다.

## 로컬 스토리지 사용하기
* 로컬 스토리지는 key-value 형태로 저장됨.
* 저장된 로컬 스토리지는 크롬 개발자 도구의 Application 탭-> Storage 메뉴에서 볼 수 있음.

```javascript
localStorage.setItem('Test-key-1', 'Test-value-1'); // 아이템 삽입
localStorage.setItem('Test-key-2', 'Test-value-2'); // 아이템 삽입
localStorage.removeItem('Test-key-1'); // 아이템 제거
localStorage.getItem('Test-key-1');  // 아이템 가져오기
localStorage.clear();  // 로컬 스토리지의 아이템을 모두 비우기
```

# References

- [Unikys](http://unikys.tistory.com/352)
- [호우님 블로그](https://vnthf.github.io/blog/localstroage/)
- [How to Use Local Storage with JavaScript](https://www.taniarascia.com/how-to-use-local-storage-with-javascript/)



