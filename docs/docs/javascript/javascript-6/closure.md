---
layout: default
title: 클로저(Closure)
parent: 6. 클로저와 스코프
grand_parent: javascript
nav_order: 3
---

# 클로저(Closure)

이미 생명 주기가 끝난 외부 함수의 변수를 참조하는 함수를 클로저라고 한다. 클로저는 함수와 함수가 선언된 어휘적 환경(lexical environment)의 조합이다. 어휘적 환경(lexical environment)이란, 선언 당시의 환경에 대한 정보를 담은 객체이다. 즉, 구성 환경이다. 예를 들어 사전에서 바나나를 검색을 하면, 바나나의 색상, 크기 등 바나나를 구성하는 정보가 나올 것이다. 같은 맥락으로, 함수의 그 함수가 **선언될 당시**의 환경정보 사이를 조합한 것을 어휘적 환경(lexical environment)이라고 한다.

여기서 선언될 당시를 하면 **스코프**가 떠오를 것이다. 클로저는 스코프와 밀접한 관련이 있다. 스코프는 변수의 유효 범위이고, 클로저는 그 유효 범위로 인한 현상이나 상태를 말한다. 스코프에서 외부로 정보를 노출시킬 수 있는 유일한 방법은 return하는 것이다. 이때 스코프 밎 어휘적 환경은 변하지 않는다. 즉, 최초 선언시의 정보를 유지한다.

클로저를 활용하면 접근 권한 제어, 지역 변수 보호, 데이터 보존 및 활용을 할 수 있다. 함수 내에서 지역변수 및 내부함수들을 생성하고, 외부에 노출시키고자 하는 멤버들로 구성된 객체를 return한다. return한 객체에 포함되지 않는 멤버들은 private하고, return한 객체에 포함된 멤버들은 public하다.

```javascript
function outerFunc(arg1, arg2){
    // 외부에서 local 값을 변경할 수 없다 (데이터 보존)
    var local = 8;
    function innerFunc(innerArg){
        console.log((arg1 + arg2)/(innerArg + local));
    }

    // innerFunc는 return했기 때문에 외부에서 접근이 가능하다
    return innerFunc;
}

var exam1 = outerFunc(2, 4);
// 외부에서 변수 local을 사용할 수 있다.
exam1(2);
```

위 예제에서는 outerFunc() 함수를 호출하고 반환하는 함수 객체인 innerFunc()가 exam1로 참조된다. 이것은 exam1(n)의 형태로 실행될 수 있다.

* outerFunc() 가 실행되면서 생성되는 변수 객체는 스코프 체인에 들어가게 된다.
* 그리고 그 스코프 체인은 innerFunc의 스코프 체인으로 참조된다.
* 즉, outerFunc() 함수가 종료되었지만, 여전히 내부 함수(innerFunc())의 [[scope]]로 참조되므로 가비지 컬렉션의 대상이 되지 않고 여전히 접근 가능하게 살아있다.
* 따라서 이후에 exam(n)을 호출하여도 innerFunc()에서 참조하고자 하는 변수 local에 접근할 수 있다.
* 이 outerFunc 변수 객체의 프로퍼티값은 실행 컨텍스트가 끝났음에도 읽기 및 쓰기까지 가능하다.

![closure](https://user-images.githubusercontent.com/16531837/44265594-3f761c80-a262-11e8-98bc-a74bc2bd1311.png)
