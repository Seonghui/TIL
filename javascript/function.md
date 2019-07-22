# 함수

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

## 4. 화살표 함수

* function을 생략해도 된다.
* 함수에 매개변수가 단 하나 뿐이라면 괄호(())도 생략할 수 있다.
* 함수 바디가 표현식 하나라면 중괄호와 return 문도 생략할 수 있다.
* 화살표 함수는 항상 익명이다. 화살표 함수도 변수에 할당할 수는 있지만 function 키워드처럼 이름이 붙은 함수를 만들 수는 없다.
* 일반 함수와 달리, this가 다른 변수와 마찬가지로 정적으로 묶인다. (즉 내부 함수 안에서 this 사용이 가능하다.)
* 객체 생성자로 사용할 수 없고, arugments 변수도 사용할 수 없다. (arguments 변수 사용 못하는건 확산연산자로 대체 가능하다.)

```js
const f1 = function() { return 'hello'; }
const f1 = () => 'hello';

const f2 = function(name) { return `hello, ${name}!`; }
const f2 = (name) => `hello, ${name}!`;

const f3 = function(a, b) { return a + b; }
const f3 = (a, b) => a + b;
```

### References

* 인사이드 자바스크립트
* 러닝 자바슼릡트