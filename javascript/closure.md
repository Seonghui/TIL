# 클로저 \(Closure\)

이미 생명 주기가 끝난 외부 함수의 변수를 참조하는 함수를 클로저라고 한다.

```javascript
function outerFunc(arg1, arg2){
    var local = 8;
    function innerFunc(innerArg){
        console.log((arg1 + arg2)/(innerArg + local));
    }

    return innerFunc;
}

var exam1 = outerFunc(2, 4);
exam1(2);
```

위 예제에서는 outerFunc\(\) 함수를 호출하고 반환하는 함수 객체인 innerFunc\(\)가 exam1로 참조된다. 이것은 exam1\(n\)의 형태로 실행될 수 있다.

* outerFunc\(\) 가 실행되면서 생성되는 변수 객체는 스코프 체인에 들어가게 된다.
* 그리고 그 스코프 체인은 innerFunc의 스코프 체인으로 참조된다.
* 즉, outerFunc\(\) 함수가 종료되었지만, 여전히 내부 함수\(innerFunc\(\)\)의 \[\[scope\]\]로 참조되므로 가비지 컬렉션의 대상이 되지 않고 여전히 접근 가능하게 살아있다.
* 따라서 이후에 exam\(n\)을 호출하여도 innerFunc\(\)에서 참조하고자 하는 변수 local에 접근할 수 있다.
* 이 outerFunc 변수 객체의 프로퍼티값은 실행 컨텍스트가 끝났음에도 읽기 및 쓰기까지 가능하다.

  ![closure](https://user-images.githubusercontent.com/16531837/44265594-3f761c80-a262-11e8-98bc-a74bc2bd1311.png)

