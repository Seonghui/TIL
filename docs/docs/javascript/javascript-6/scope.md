---
layout: default
title: 스코프
parent: 6. 클로저와 스코프
grand_parent: javascript
nav_order: 2
---

# 스코프 \(Scope\)

## 1. 스코프\(Scope\)

스코프의 사전적 정의는 **범위**라는 뜻이다. 자바스크립트도 다른 언어와 마찬가지로 유효한 범위, 즉 **스코프**가 있다. 대부분의 프로그래밍 언어는 적당한 형식의 범위를 가지며, 그 차이는 해당 범위가 얼마나 지속되는가 하는 점이다  
여기서 범위란 어떤 범위를 말하는 걸까? 바로 **변수와 매개변수\(Parameter\)의 접근성과 생존기간**을 뜻한다. 따라서 스코프의 개념을 잘알고 있다면 변수와 매개변수의 접근성과 생존기간을 제어할 수 있다.

![scope](https://user-images.githubusercontent.com/16531837/43557724-0aa62a54-9641-11e8-9ad7-3c7512b64581.png)

### 1.1 자바스크립트 스코프 특징

자바스크립트는 전역 범위의 스코프와 함수 단위의 스코프를 가지는데, 대부분의 다른 언어가 가지고 있는 블록 단위 스코프가 아닌, **함수 단위 스코프**를 가지고 있다. 여기서 블록이란 코드 내에서 중괄호{}로 둘러싸인 부분을 가리킨다. 따라서 블록 단위 스코프의 경우 if, for 구문의 블록 안에 선언된 변수는 밖에서 접근이 불가능하지만, 자바스크립트는 함수 단위 스코프를 가지기 때문에 if나 for 구문은 유효 범위가 없다. 오직 함수만이 유효 범위의 한 단위가 된다. \(하지만 ECMAScript6 에서는 함수 단위 스코프인 `var` 이외에, `let` 이라는 블록 단위 스코프 변수 선언을 지원한다.\)

참고로 스코프는 정의될 때 결정된다. 절대 실행이 아니다.

* 함수 단위의 스코프
* 변수명 중복 허용

```javascript
var a = "전역 변수 A";

function A() {
  var a = "지역 변수 A";
  console.log("A 함수 출력 결과: " + a);
  function B() {
    console.log("B 함수 출력 결과: " + a);
  }
  B();
}

A();
console.log(a);
```

다른 프로그래밍 언어들은 유효범위의 단위기 블록 단위이기 때문에, 위의 코드와 같이 B 함수에서 변수 a 를 사용할 수 없다. 하지만 **자바스크립트의 유효범위는 함수 단위**이기 때문에 B 함수에서 변수 a 를 사용할 수 있는 것이다.  
또한 자바스크립트는 다른 프로그래밍 언어와 달리 **변수명이 중복되어도 에러가 나지 않는다.** 단, 같은 변수명이 여러 개 있는 변수를 참조할 때 가장 가까운 범위의 변수를 참조한다.  
위의 코드 화면을 보면 B 함수에서 변수 a 를 참조할 때, 전역 변수 A 가 아닌 A 함수 안에 있는 지역 변수를 참조하는 것을 알 수 있다.

### 1.2 전역 스코프와 지역 스코프

스코프에는 두 가지 종류가 있다. 하나는 전역 스코프, 또 하나는 지역 스코프이다.

* 전역 스코프
  * 스크립트 전체에서 참조되는 것을 의미한다. 말 그대로 스크립트 내 어느 곳에서든 참조된다. \(전역 변수\)
* 지역 스코프
  * 정의된 함수 안에서 참조되는 것을 의미한다. 함수 밖에서는 참조하지 못한다. \(지역 변수\)

## 2. 스코프 체인

### 2.1 전역 실행 컨텍스트의 스코프 체인

유효 범위를 나타내는 스코프가 실행 컨택스트 내에서 \[\[scope\]\] 프로퍼티\(연결 리스트 형식\)로 관리되는데, 이를 스코프 체인이라고 한다. 각각의 함수는 \[\[scope\]\] 프로퍼티로 자신이 생성된 실행 컨텍스트의 스코프 체인을 참조한다. 함수가 실행되는 순간 실행 컨텍스트가 만들어지고, 이 실행 컨텍스트는 실행된 함수의 \[\[scope\]\] 프로퍼티를 기반으로 새로운 스코프 체인을 만든다.

* 각 함수 객체는 \[\[scope\]\] 프로퍼티로 현재 컨텍스트의 스코프 체인을 참조한다.
* 한 함수가 실행되면 새로운 실행 컨텍스트가 만들어지는데, 이 새로운 실행 컨텍스트는 자신이 사용할 스코프 체인을 다음과 같은 방법으로 만든다. - 현재 실행되는 함수 객체의 \[\[scope\]\] 프로퍼티를 복사하고, 새롭게 생성된 변수 객체를 해당 체인의 맨 앞에 추가한다.

```javascript
var var1 = 1;
var var2 = 2;
console.log(var1);  // 1
console.log(var2);  // 2
```

위 예제는 전역 코드이다. 함수가 선언되지 않아 함수 호출이 없고 실행 가능한 코드만 나열되어 있다.  
이 자바스크립트 코드를 실행하면 먼저 전역 실행 컨텍스트가 생성되고, 변수 객체가 만들어진다. 그렇다면 이 변수 객체의 스코프체인은 어떻게 될까?  
현재 전역 실행 컨텍스트 단 하나만 실행되고 있어 참조할 상위 컨텍스트가 없다. 따라서 자신이 최상위에 위치하는 변수 객체가 되어, 변수 객체의 스코프 체인은 자기 자신만을 가진다. 즉, 변수 객체의 \[\[scope\]\]는 변수 객체 자신을 가리킨다. 

![scope-chain-1](https://user-images.githubusercontent.com/16531837/44142762-30c2dea0-a0bc-11e8-8b30-92740aa1280e.png)

### 2-2. 함수를 호출할 경우 생성되는 실행 컨텍스트의 스코프 체인

```javascript
var var1 = 1;
var var2 = 2;
function func() {
    var var1 = 10;
    var var2 = 20;
    console.log(var1); // 10
    console.log(var2); // 20
}
func();
console.log(var1);  // 1
console.log(var2);  // 2
```

위 예제를 실행하면 전역 실행 컨텍스트가 생성되고 func\(\) 함수 객체가 만들어진다. 이 함수 객체의 \[\[scope\]\]는 어떻게 될까?  
함수 객체가 생성될 때, 그 함수 객체의 \[\[scope\]\]는 현재 실행되는 변수 객체에 있는 \[\[scope\]\]를 그대로 가진다. 따라서 func 함수 객체의 \[\[scope\]\]는 전역 변수 객체가 된다.

![scope-chain-2](https://user-images.githubusercontent.com/16531837/44143156-8b895a84-a0bd-11e8-8efb-3de7391998dd.png)

함수를 실행하면 새로운 컨텍스트\(func 컨텍스트\)가 만들어진다. func 컨텍스트의 스코프 체인은 실행된 함수의 \[\[scope\]\] 프로퍼티를 그대로 복사한 후, 현재 생성된 변수 객체를 복사한 스코프 체인의 맨 앞에 추가한다.

### References

* [Nextree 포스트](http://www.nextree.co.kr/p7363/)

