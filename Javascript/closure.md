클로저
===
외부 함수의 변수에 접근할 수 있는 내부 함수를 일컫는다. 스코프 체인이라고 표현되기도 한다.
* 클로저 자신에 대한 접근
* 외부 함수의 변수에 대한 접근
* 전역 변수에 대한 접근
이렇게 세 단계로 구분할 수 있다.  
내부 함수는 외부 함수의 변수뿐만 아니라 파라미터에도 접근할 수 있다. 단, 내부 함수는 외부 함수의 arguments 객체를 호출할 수는 없다.

```javascript
function showName(firstName, lastName) {
    var nameIntro = "Your name is ";
    // 이 내부 함수는 외부함수의 변수뿐만 아니라 파라미터 까지 사용할 수 있습니다.
    function makeFullName() {
        return nameIntro + firstName + " " + lastName;
    }
    return makeFullName();
}
showName("Michael", "Jackson"); // Your name is Michael Jackson
```