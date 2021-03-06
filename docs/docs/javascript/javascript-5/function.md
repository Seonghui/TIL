---
layout: default
title: 함수의 정의
parent: 5. 함수
grand_parent: javascript
nav_order: 1
---

# 함수의 정의

{: .no_toc }
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 1. 함수의 정의

자바스크립트의 함수는 언뜻 보면 여느 언어와 마찬가지의 기능을 제공한다. 즉, 특정 기능을 제공하는 코드를 작성해서 함수를 정의하고, 이를 호출해서 결과값을 얻는 것처럼 말이다. 하지만 이러한 기본적인 기능 외에도, 자바스크립트 함수를 제대로 이해하려면 함수가 일급 객체이며 함수가 일반 객체처럼 값으로 취급된다는 것을 이해해야 한다.

* 리터럴에 의해 생성
* 변수나 배열의 요소, 객체의 프로퍼티 등에 할당 가능
* 함수의 인자로 전달 가능
* 함수의 리턴값으로 리턴 가능
* 동적으로 프로퍼티를 생성 및 할당 가능  

자바스크립트의 함수는 일반 객체처럼 값으로 취급한다. 때문에 함수는 위와 같은 동작이 가능하다. 이러한 동작이 전부 가능한 객체를 프로그래밍에서는 일급 객체라고 부른다.

## 2. 함수 생성방법

자바스크립트에서 함수를 생성하는 방법은 3 가지가 있다. 모두 같은 함수를 생성하지만 각가의 방식에 따라 함수가 동작하는 방식이 미묘하게 차이가 난다.

* 함수 선언문
* 함수 표현식
* Function\(\) 생성자 함수

### 2.1 함수 선언문

* 반드시 함수명이 정의되어 있어야 한다.

```javascript
function add(x, y) {
  return x + y;
}

add(3, 4); //7
```

사실 위와 같은 함수는 자바스크립트 인터프리터에 의해 이렇게 해석된다. 이때문에 add\(\)로 함수를 호출할 수 있는 것이다.

```javascript
var add = function add(x, y) {
  return x + y;
};

add(3, 4); //7
```

### 2.2 함수 표현식

자바스크립트에서는 함수도 하나의 값처럼 취급되기 때문에, 함수도 숫자나 문자열처럼 변수에 할당하는 것이 가능하다. 이런 방식으로, 함수 리터럴로 하나의 함수를 만들고 이 함수를 변수에 할당하여 함수를 생성하는 것을 함수 표현식이라고 부른다.

#### 2.2.1 익명 함수 표현식

함수 리터럴로 만든 함수에 이름이 없다. 여기서 add 와 plus 변수는 같은 익명 함수를 참조하고 있다.

```javascript
var add = function(x, y) {
  return x + y;
};

var plus = add;

add(3, 4); //7
plus(5, 6); //11
```

#### 2.2.2 기명 함수 표현식

함수 리터럴로 만든 함수에 이름이 있다. 이런 경우에는 재귀적인 호출 처리가 가능하다.

```javascript
var add = function sum(x, y) {
  return x + y;
};

add(3, 4); //7
sum(5, 6); //에러
```

### 2.3 Function\(\) 생성자 함수

자주 사용되지 않으니 상식 수준으로만 알아두자.

```javascript
var add = new Function("x", "y", "return x + y");
add(3, 4); //7
```

## 3. 함수의 기능

함수는 [일급 객체](https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B8%89_%EA%B0%9D%EC%B2%B4)이기 때문에 다음과 같은 동작이 가능하다.

### 3.1 변수나 프로퍼티의 값으로 할당

```javascript
// 변수에 함수 할당
var bar = function() {
  return 100;
};
console.log(bar());

//프로퍼티에 함수 할당
var obj = {};
obj.baz = function() {
  return 200;
};
console.log(obj.baz());
```

### 3.2 함수 인자로 전달

```javascript
var foo = function(func) {
  func();
};

foo(function() {
  console.log("함수는 인자로 사용될 수 있다.");
});
```

### 3.3 리턴값으로 활용

```javascript
var foo = function() {
  return function() {
    console.log("return value");
  };
};

var bar = foo();
bar();
var foo = function() {
  return function() {
    console.log("return value");
  };
};

var bar = foo();
bar();
```

### 3.4 함수 객체의 기본 프로퍼티

함수 역시 객체이다. 그래서 함수 역시 일반적인 객체의 기능에 추가로 호출됐을 때 정의된 코드를 실행하는 기능을 가지고 있다. 또한, 일반 객체와는 다르게 함수 객체만의 표준 프로퍼티가 정의되어 있다. 예를 들면 함수 객체의 표준 프로퍼티인 length 프로퍼티가 있다.

## 4. 콜백함수

콜백은 다른 함수가 실행을 끝낸 뒤 실행되는 — call back 되는 함수를 말한다. 자세히 말하자면, 자바스크립트에서 함수는 일급 객체이며 object이다. 이 때문에 함수는 다른 함수의 인자로 쓰일 수도, 어떤 함수에 의해 리턴될 수도 있다. 이러한 함수를 고차 함수 higher-order functions 라 부르고 인자로 넘겨지는 함수를 콜백 함수 callback function 라고 부른다.

### 4.1 콜백함수의 특징

다른 함수(A)의 매개변수로 콜백함수(B)를 전달하면, A가 B의 제어권을 갖는다. 그리고 특별한 요청이 없는 한 A에 미리 정해진 방식에 따라 B를 호출한다. 여기서 말하는 미리 정해진 방식이란, this에 무엇을 바인딩할지, 매개 변수에는 어떤 값들을 지정할지, 어떤 타이밍에 콜백을 호출할지 등이다. (콜백 함수는 전달 받은 즉시 바로 실행이 될 필요가 없다. 함수의 내부의 어느 특정시점에 실행을 한다.)

```js
var arr = [1, 2, 3, 4, 5]
arr.forEach(function(index, value) {
  console.log(index, value)
})
```

위 예제를 보면, 콜백 함수에 index, value 인자를 받아 출력하면 앞뒤가 뒤바뀐 채로 출력이 된다. 왜냐하면 콜백함수를 정의할 때 파라메터의 순서는 제어권을 넘겨받을 대상, 즉 forEach의 규칙을 따르게 된다.

### 4.2 콜백함수는 클로저다

우리가 다른 함수의 인자로 콜백함수를 전달할 때, 전달받은 함수의 특정시점에 그 콜백함수가 동작을 할 것이다. 마치 전달받은 함수가 이미 콜백함수를 내부에서 정의 한 것처럼 말이다. 이 말은 콜백은 클로저라는 말과 같다.

간단하게 설명을 하면 전달된 콜백함수는 콜백함수를 포함한 함수 내부의 인자에 접근이 가능하고 심지어 전역변수에도 접근이 가능한 상태가 된다. 즉, 기존의 함수가 가진 스코프에 새로운 내부 스코프가 추가가 되는 것이다.
클로저는 포함하고 있는 함수의 스코프에 접근을 할수 있다. 그래서 콜백함수는 포함하고 있는 함수의 변수에 접근이 가능하고 심지어 전역변수에도 접근이 가능하다.

## 5. ES6에서의 함수

### 5-1. 화살표 함수

* function을 생략해도 된다.
* 함수에 매개변수가 단 하나 뿐이라면 괄호()도 생략할 수 있다.
* 함수 바디가 표현식 하나라면 중괄호와 return 문도 생략할 수 있다.
* 화살표 함수는 항상 익명이다. 화살표 함수도 변수에 할당할 수는 있지만 function 키워드처럼 이름이 붙은 함수를 만들 수는 없다.
* 일반 함수와 달리, this가 다른 변수와 마찬가지로 정적으로 묶인다. (즉 내부 함수 안에서 this 사용이 가능하다.) 어디에 묶이냐? 바로 직전에 있는 스코프에 묶인다
* 객체 생성자로 사용할 수 없고, arugments 변수도 사용할 수 없다. (arguments 변수 사용 못하는건 확산연산자로 대체 가능하다.)
* 화살표 함수에는 없는 것: 함수 이름, this, arguments

```javascript
const f1 = function() { return 'hello'; }
const f1 = () => 'hello';

const f2 = function(name) { return `hello, ${name}!`; }
const f2 = (name) => `hello, ${name}!`;

const f3 = function(a, b) { return a + b; }
const f3 = (a, b) => a + b;
```

### 5-2. Parameter 기본값 설정

ES6부턴 함수 파라미터에 기본값을 설정할 수 있다.

```js
// ES5
function add(x, y) {
  x = x || 0 // 매개변수 x에 인수를 할당하지 않은 경우, 기본값 0을 할당
  y = y || 0 // 매개변수 y에 인수를 할당하지 않은 경우, 기본값 0을 할당

  return x + y
}

console.log(add())     // 0
console.log(add(1, 2)) // 3
```

```js
// ES6
const add = (x = 0, y = 0) => {
  // 파라미터 x, y에 인수를 할당하지 않은 경우, 기본값 0을 할당
  return x + y
}

console.log(add())     // 0
console.log(add(1, 2)) // 3
```

### 5-3. Rest Parameter

Rest 파라미터를 사용하면 함수의 파라미터로 오는 값들을 "배열"로 전달받을 수 있다. 함수의 마지막 파라미터의 앞에 `...` 를 붙여 모든 나머지 인수를 배열로 대체한다. 마지막 파라미터만 **Rest 파라미터** 가 될 수 있다.

```js
function myFun(a, b, ...manyMoreArgs) {
  console.log("a", a); 
  console.log("b", b);
  console.log("manyMoreArgs", manyMoreArgs); 
}

myFun("one", "two", "three", "four", "five", "six");

// Console Output:
// a, one
// b, two
// manyMoreArgs, [three, four, five, six]
```

### References

* 인사이드 자바스크립트
* 러닝 자바스크립트
* [[번역] JavaScript: 도대체 콜백이 뭔데?](https://medium.com/@oasis9217/%EB%B2%88%EC%97%AD-javascript-%EB%8F%84%EB%8C%80%EC%B2%B4-%EC%BD%9C%EB%B0%B1%EC%9D%B4-%EB%AD%94%EB%8D%B0-65bb82556c56)
* [[자바스크립트] 자바스크립트의 콜백함수 이해하기!_v2](https://yubylab.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-%EC%BD%9C%EB%B0%B1%ED%95%A8%EC%88%98-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)
