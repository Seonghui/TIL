---
layout: default
title: 변수
parent: 2. 변수, 상수, 데이터 타입
grand_parent: javascript
nav_order: 1
---

# 변수 (Variable)

{: .no_toc }
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# 1. 변수, 상수, 리터럴

## 1-1. 변수

변수란 간단히 말해 이름이 붙은 값으로, 언제든 바뀔 수 있다. ES6 이후부터는 let 키워드로 선언한다.

```js
// 변수 선언
// ES5 이전에는 var 키워드로 선언
let age = 20;

// let 키워드로 선언한 변수를 변경
age = 18;
```

## 1-2. 상수

변수와 마찬가지로 값을 할당받을 수 있지만, 한 번 할당한 값은 바꿀 수 없다. ES6에서 새로 생긴 개념으로 const 키워드로 생성한다.

```js
const gender = '여자';
```

## 1-3. 리터럴과 식별자

* 리터럴(literal): 변수 및 상수에 저장되는 **값 자체**를 의미한다.
* 식별자(Identifier): 변수를 가리키는 식별자이다.

```js
// 식별자: gender
// 리터럴: 여자 (문자형 리터럴)
const gender = "여자";

// 식별자: age
// 리터럴: 20 (숫자형 리터럴)
let age = 20;
```

# 2. 변수의 개념

* 자바스크립트 변수는 값을 복사하거나 참조하는 방식으로 데이터를 담는다.
* 기본이 되는 값은 무엇이든 변수로 복제한다.
* 할당되고 복사되고 함수에 전달하거나 반환할 때 값으로 처리된다.
* 이외에는 참조하는 방식인데, 값을 담지 않는다면 객체를 참조한다.
* 여기서 참조값은 객체가 메모리에 담긴 주소값을 가리키는 포인터이다.
* 단, 문자열을 변경시에는 문자열 수정이 아닌 새로운 객체가 된다. (2-5번 예시 참고)

## 2-1. 초기값을 할당하지 않으면 undefined

```js
let job;
console.log(job) // undefined
```

## 2-2. let 문 하나에서 변수 여러 개 선언하기

```js
let name, interest = '게임', address = '서울';

console.log(name); // undefined
console.log(interest); // 게임
console.log(address); // 서울
```

## 2-3. 단일 객체를 여러 변수가 참조하는 예시

* 객체의 실제 값을 복사하지 않은 채, 단순히 똑같은 객체를 가리키는 경우이다.
* 따라서 두 개의 변수가 똑같은 객체를 가리키고 있다.
* 한 개의 참조 값을 통해 참조하는 값을 갱신하면 다른 참조 값에도 반영된다.

```javascript
// 객체 생성
var obj = {};

// 객체 참조
var refToObj = obj;

// 객체 속성 변경
obj.onProperty = true;

console.log(refToObj.onProperty === obj.onProperty); // true

// obj 객체를 참조한 변수에 프로퍼티 추가 (갱신)
refToObj.anotherProperty = 1;

console.log(refToObj.anotherProperty === obj.anotherProperty); // true
```

## 2-4. 객체의 참조값을 수정했지만 본래의 상태를 유지하는 경우

* 참조 값은 다른 참조 값이 아니라 오로지 참조되는 객체만을 가리킨다
* 이게 무슨 말이냐면, 객체 실체가 바뀌었지만 참조한 값은 여전히 이전의 객체를 가리키는 경우이다.

```javascript
// 배열 생성
var items = ['one', 'two', 'three'];
// 참조
var itemRef = items;

// items 를 다른 객체로 변경
items = ['new', 'array'];

console.log(items !== itemRef); // true

console.log(items); // ['new', 'array']
console.log(itemRef); // ['one', 'two', 'three']
```

## 2-5. 문자열 객체를 수정하자 새로운 객체가 만들어진 예시

```javascript
// 문자열 객체 생성
var item = 'test';

// 참조
var itemRef = item;


// 수정
// 이렇게 수정하면 원시 객체를 수정하지 않고 새로운 객체가 만들어짐
item += 'ing';

console.log(item != itemRef); // true
```

# 3. 전역 변수와 지역 변수

## 3-1. 전역 변수 (Global Variable)

* 자바스크립트에서 제일 바깥 단위에 선언하는 변수.
* 함수에 포함되지 않는다.
* 어디에서나 사용이 가능하다.
* 사용을 자제하는 게 좋다. (보안 등의 이유)

## 3-2. 지역 변수 (Local Variable)

* 함수 내부에 선언된 변수로, 함수가 실행되면 만들어지고 함수가 종료되면 소멸하는 변수이다.
* 함수 외부에서는 접근할 수 없다.

## 3-3. 예제

```javascript
var a = "전역 변수";

function myFunc() {
  var b = "지역 변수";
  console.log(b);
}

myFunc();
console.log(a);
```

여기서 a 는 전역 변수, b 는 지역 변수이다.

# 4. var, const, let

* var와 다르게 변수 재선언 불가능이다.
* `var`는 function-scoped이고, `let`, `const`는 block-scoped이다.
* var와 let/const선언에 대한 범위의 차이 중 하나는 let/const가 TDZ에 의해 제약을 받는다는 것이다.
* 즉, 변수가 초기화되기 전에 액세스하려고 하면, var처럼 undefined를 반환하지 않고, ReferenceError가 발생한다. 이는 코드를 예측가능하고 잠재적 버그를 쉽게 찾아낼 수 있도록 한다.

## 4-1. const와 let의 immutable 여부

* `const`는 선언과 동시에 반드시 할당을 해야 한다. `let`은 그렇지 않다.
* `const`와 `let`의 차이점은 변수의 immutable 여부이다. `let`은 변수에 재할당이 가능하지만, `const`는 변수 재선언, 재할당 모두 불가능하다.

```js
// let
let a = 'test'
let a = 'test2' // Uncaught SyntaxError: Identifier 'a' has already been declared
a = 'test3'     // 가능

// const
const b = 'test'
const b = 'test2' // Uncaught SyntaxError: Identifier 'a' has already been declared
b = 'test3'    // Uncaught TypeError:Assignment to constant variable.
```

## 4-2. const와 let의 호이스팅

`var`는 호이스팅이 된다.

그렇다고 `let`, `const`가 hoisting이 발생하지 않는건 아니다. `var`가 **function-scoped**로 hoisting이 되었다면, `let`, `const`는 **block-scoped**단위로 hoisting이 일어나는데 아래 코드에서 ReferenceError가 발생한 이유는 tdz(temporal dead zone)때문이다.

let은 값을 할당하기전에 변수가 선언 되어있어야 하는데 그렇지 않기 때문에 에러가 난다. 변수가 초기화되기 전에 액세스하려고 해서 그렇다.

```js
c = 'test' // ReferenceError: c is not defined
let c
```

이건 const도 마찬가지인데 좀 더 엄격하다.

```js
// let은 선언하고 나중에 값을 할당이 가능하지만
let dd
dd = 'test'

// const 선언과 동시에 값을 할당 해야한다.
const aa // Missing initializer in const declaration
```

let/const변수는 초기화 구문이 완전히 실행된 이후에 우변이 실행되고 그 결과 값이 선언된 변수에 할당된 후 초기화된다. 우변에서 let/const로 선언된 변수를 사용하는 경우, 우변은 변수를 읽으려고 시도하지만 초기화 구문이 아직 완전히 실행되지 않은 상태이므로, ReferenceError가 발생한다.

따라서 let/const선언 변수는 호이스팅되지 않는 것이 아니다. 스코프에 진입할 때 변수가 만들어지고 TDZ(Temporal Dead Zone)가 생성되지만, 코드 실행이 변수가 실제 있는 위치에 도달할 때까지 액세스할 수 없는 것이다.

## 4-3. TDZ(Temporal Dead Zone)

let/const선언은 실행중인 실행 컨텍스트의 어휘적 환경(Lexical Environment)으로 범위가 지정된 변수를 정의한다. 변수는 그들의 어휘적 환경에 포함될 때 생성되지만, 어휘적 바인딩이 실행되기 전까지는 액세스할 수 없다. 새로운 범위에 진입할 때마다 지정된 범위에 속한 모든 let/const바인딩이 지정된 범위 내부의 코드가 실행되기 전에 실행된다. (즉, let/const선언이 호이스팅된다.) 어휘적 바인딩이 실행되기 전까지 액세스할 수 없는 현상을 TDZ라고 한다.

```js
// const x를 실행하기 전에 x에 접근하면, TDZ에 의해 ReferenceError가 발생하게 된다.
// console.log(x);
const x = 42;
// 위 코드 실행 이후에는 x에 접근할 수 있다.
console.log(x);
```

어휘적 바인딩에 의해 초기화되며 정의된 변수는 변수가 만들어질 때가 아닌, 값이 해당 초기화 구문 어휘적 바인딩이 실행될 때 값을 할당받는다. let/const선언의 어휘적 바인딩에 초기화 구문이 없으면, 어휘적 바인딩을 실행할 때, 변수에 undefined가 할당된다.

```js
let x; // 이는 let x = undefined; 와 같다.
// const 키워드는 반드시 선언과 동시에 값을 할당해야 한다.
// const x; Uncaught SyntaxError: Missing initializer in const declaration
```

let/const변수는 초기화 구문이 완전히 실행된 이후에 우변이 실행되고 그 결과 값이 선언된 변수에 할당된 후 초기화된다. 우변에서 let/const로 선언된 변수를 사용하는 경우, 우변은 변수를 읽으려고 시도하지만 초기화 구문이 아직 완전히 실행되지 않은 상태이므로, ReferenceError가 발생한다

```js
const x = x; // ReferenceError
const a = f(); // ReferenceError
const b = 2;
function f() {
  return b;
}
```

# References

* 모던 자바스크립트 테크닉
* [var, let, const 차이점은?](https://gist.github.com/LeoHeo/7c2a2a6dbcf80becaaa1e61e90091e5d)
* [let과 const는 호이스팅 될까?](https://medium.com/korbit-engineering/let%EA%B3%BC-const%EB%8A%94-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85-%EB%90%A0%EA%B9%8C-72fcf2fac365)
