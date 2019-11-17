---
layout: default
title: 자바스크립트의 객체
parent: 7. 객체지향 프로그래밍
grand_parent: javascript
nav_order: 1
---

# 자바스크립트의 객체

{: .no_toc }

{: .no_toc .text-delta }

1. TOC
{:toc}

---

객체는 관련된 데이터와 함수(메서드)의 집합이다. 이 집합은 키(Key)와 값(Value)으로 구성어있으며, 데이터와 함수를 프로퍼티라고 부른다. 아무것도 없는 빈 객체를 만들어보자

```js
var person = {}; // [object Object]
```

이 객체를 다음과 같이 채울 수 있다.

```js
var person = {
  name: 'baz',
  age: 20
};
```

## 1. 객체 생성 방법

1. 객체 리터럴
2. Object 생성자 함수
3. 생성자 함수

크게 세 가지로 나뉘어진다.

### 1-1. 객체 리터럴

가장 일반적인 자바스크립트의 객체 생성 방식이다. 클래스 기반 객체 지향 언어와 비교할 때 매우 간편하게 객체를 생성할 수 있다. 중괄호({})를 사용하여 객체를 생성하는데 {} 내에 1개 이상의 프로퍼티를 기술하면 해당 프로퍼티가 추가된 객체를 생성할 수 있다. {} 내에 아무것도 기술하지 않으면 빈 객체가 생성된다.

```js
var emptyObject = {};

var person = {
  name: 'baz',
  age: 20,
  sayHello: function() {
    console.log('My name is ' + this.name);
  }
};

person.sayHello() // My name is baz
```

### 1-2. Obejct 생성자 함수

new 연산자와 Object 생성자 함수를 호출하여 빈 객체를 생성할 수 있다. 빈 객체 생성 이후 프로퍼티 또는 메소드를 추가하여 객체를 완성하는 방법이다. 객체가 소유하고 있지 않은 프로퍼티 키에 값을 할당하면 해당 객체에 프로퍼티를 추가하고 값을 할당한다. 이미 존재하는 프로퍼티 키에 새로운 값을 할당하면 프로퍼티 값은 할당한 값으로 변경된다.

```js
var person = new Object();
person.name = 'baz';
person.age = 20;
person.sayHello = function() {
  console.log('My name is ' + this.name);
}

person.sayHello() // My name is baz
```

반드시 Object 생성자 함수를 사용해 빈 객체를 생성해야 하는 것은 아니다. 객체를 생성하는 방법은 객체 리터럴을 사용하는 것이 더 간편하다. Object 생성자 함수 방식은 특별한 이유가 없다면 그다지 유용해 보이지 않는다.

사실 **객체 리터럴 방식으로 생성된 객체는 결국 빌트인(Built-in) 함수인 Object 생성자 함수로 객체를 생성하는 것을 단순화시킨 축약 표현(short-hand)이다.** 다시 말해, 자바스크립트 엔진은 객체 리터럴로 객체를 생성하는 코드를 만나면 내부적으로 Object 생성자 함수를 사용하여 객체를 생성한다. 따라서 개발자가 일부러 Object 생성자 함수를 사용해 객체를 생성해야 할 일은 거의 없다.

### 1-3. 생성자 함수

생성자 함수를 사용하면 프로퍼티가 동일한 객체 여러 개를 간편하게 생성할 수 있다. 자세한 건 ![이 링크]({{site.url}}/TIL/docs/javascript/javascript-5/constructor-function.html) 참고.

## 2. 객체의 프로퍼티에 접근하는 방법

### 2-1. 점 표기법

객체의 프로퍼티와 메서드는 점 표기법으로 접근이 가능하다. 객체 내에 캡슐화되어있는 것에 접근하려면 먼저 점을 입력한 다음 접근하고자 하는 항목을 적는다.

```js
var person = {
  name: 'baz',
  age: 20
};

console.log(person.age); // 20
```

### 2-2. 괄호 표기법

person.age를 괄호 표기법으로 접근하려면 다음과 같이 사용하면 된다.

```js
var person = {
  name: 'baz',
  age: 20
};

console.log(person['age']); // 20
```

## 3. 프로퍼티 동적 생성

괄호 표기법과 점 표기법으로 프로퍼티 추가가 가능하다. 객체가 소유하고 있지 않은 프로퍼티 키에 값을 할당하면 하면 주어진 키와 값으로 프로퍼티를 생성하여 객체에 추가한다.

```js
var person = {
  name: 'baz',
  age: 20
};

person.gender = 'woman';
person['eyes'] = 'blue';

console.log(person);; // {name: "baz", age: 20, gender: "woman", eyes: "blue"}
```

## 4. 프로퍼티 삭제

`delete` 연산자를 사용하면 객체의 프로퍼티를 삭제할 수 있다. 이때 피연산자는 프로퍼티 키이어야 한다.

```js
var person = {
  'first-name': 'Ung-mo',
  'last-name': 'Lee',
  gender: 'male',
};

delete person.gender;
console.log(person.gender); // undefined

delete person;
console.log(person); // Object {first-name: 'Ung-mo', last-name: 'Lee'}
```

## References

* [자바스크립트 객체 기본](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/Basics)
