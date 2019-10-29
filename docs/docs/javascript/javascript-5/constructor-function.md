---
layout: default
title: 생성자 함수
parent: 5. 함수
grand_parent: javascript
nav_order: 2
---

# 생성자 함수

## 1. 생성자 함수의 개념

객체란 서로 연관된 변수와 함수를 그룹핑한 그릇이라고 할 수 있다. 객체 내의 변수를 프로퍼티(property), 함수를 메소드(method)라고 부른다. 객체를 만들어보자.

```js
var person = {}
person.name = 'Stella';
person.introduce = function(){
    return 'My name is '+this.name;
}
document.write(person.introduce());
```

위 코드는 아래와 같이 바꿀 수 있다.

```js
var person = {
    'name' : 'egoing',
    'introduce' : function(){
        return 'My name is '+this.name;
    }
}
document.write(person.introduce());
```

위 코드는 객체를 정의할 때 값을 세팅하게 된다. 지금은 name이 이미 stella로 값이 정해져 있다. 만약 다른 사람의 이름을 담을 객체가 필요하다면 객체의 정의를 반복해야 하는데 이는 매우 비효율적이다. 객체의 구조를 재활용할 수 있는 방법이 필요한데, 이때 사용하는 것이 생성자다.

생성자 함수는 말 그대로 자바스크립트의 객체를 생성하는 역할을 하는 함수이다. 하지만 여느 언어와는 달리 그 형식이 정해져 있지 않고, 기존 함수를 new 로 호출하면 해당 함수는 생성자 함수로 동작한다. 생성자 함수를 통해 생성된 객체를 인스턴스라고 한다.

```javascript
function Person(name) {
  this.name = name;
}
var stella = new Person("Stella");
console.log(stella);
```

이는 반대로 생각하면 일반 함수에 new 를 붙여 호출하면 원치 않은 생성자 함수처럼 동작할 수 있다. 따라서 대부분의 자바스크립트 스타일 가이드에서는 생성자 함수의 함수 이름의 첫 문자를 대문자로 쓰기를 권하고 있다. 되도록이면 생성자 함수 이름은 대문자(파스칼 케이스, PascalCase)로 쓰자.

### new 키워드

생성자를 호출할 때에는 항상 new 키워드를 이용해야 한다. new 키워드를 이용해 생성자 함수를 호출하면 컨텍스트가 전역(윈도우)에서 만들려고 하는 인스턴스 전용의 새로운 빈 컨텍스트로 바뀐다.

### 생성자 함수의 특징

* 프로퍼티 또는 메소드명 앞에 기술한 this는 생성자 함수가 생성할 인스턴스(instance)를 가리킨다.
* this에 연결(바인딩)되어 있는 프로퍼티와 메소드는 public(외부에서 참조 가능)하다.
* 생성자 함수 내에서 선언된 일반 변수는 private(외부에서 참조 불가능)하다. 즉, 생성자 함수 내부에서는 자유롭게 접근이 가능하나 외부에서 접근할 수 없다.

```js
function Person(name, gender) {
  var married = true;         // private
  this.name = name;           // public
  this.gender = gender;       // public
  this.sayHello = function(){ // public
    console.log('Hi! My name is ' + this.name);
  };
}

var person = new Person('Lee', 'male');

console.log(typeof person); // object
console.log(person); // Person { name: 'Lee', gender: 'male', sayHello: [Function] }

console.log(person.gender);  // 'male'
console.log(person.married); // undefined
```

## 2. 생성자 함수 new 를 붙이지 않고 호출할 경우

일반 함수 호출과 생성자 함수를 호출할 때 this 바인딩 방식이 다르기 때문에 오류가 발생할 수 있다.

* 일반 함수 호출시: this 가 window 전역 객체에 바인딩됨.
* 생성자 함수 호출시: this 가 새로 생성되는 빈 객체에 바인딩됨.

```js
function Person(name, age, gender, position) {
  this.name = name;
  this.age = age;
  this.gender = gender;
}

// new를 붙이지 않고 호출한 경우
var bar = Person("bar", 20, "man");
console.log(bar); // undefined
console.log(window.name); // qux
console.log(window.age); // 20
console.log(window.gender); // man

// new를 붙여서 호출한 경우
var barz = new Person("barz", 30, "woman");
console.log(barz); // Person 객체
console.log(barz.name); // barz
console.log(barz.age); // 30
console.log(barz.gender); // woman
```

## 3. 생성자 함수가 동작하는 방식

new 연산자로 자바스크립트 함수를 생성자로 호출하면, 다음과 같은 순서로 동작한다.

![생성자 함수]({{site.url}}/TIL/assets/images/javascript/javascript-5/constructor-function-1.png)

1. new 키워드는 빈 객체{}를 생성
2. 생성자 함수는 빈 객체에 생성할 프로퍼티를 정의
3. 함수의 인수들을 프로퍼티에 할당
4. 생성된 Student 객체를 student에 할당. 이때 새 객체의 프로토타입 링크는 자신을 생성한 함수의 프로토타입 오브젝트를 참조한다.

![constructor](https://user-images.githubusercontent.com/16531837/43557706-f2da1624-9640-11e8-8727-cd9ea75cf672.png)

## 4. 객체 리터럴 방식 vs 생성자 함수 방식

* 객체 리터럴 방식
  * 같은 형태의 객체를 재생성할 수 없음
  * 프로토타입 링크가 가리키는 프로토타입 객체가 Object
* 생성자 함수 방식
  * 같은 형태의 서로 다른 객체 생성 가능
  * 프로토타입 링크가 가리키는 프로토타입 객체가 생성자 함수의 프로토타입 오브젝트

```javascript
//객체 리터럴 방식으로 foo 객체 생성
var foo = {
  name: "foo",
  age: 35,
  gender: "man"
};
console.dir(foo);

//생성자 함수
function Person(name, age, gender, position) {
  this.name = name;
  this.age = age;
  this.gender = gender;
}

// Person 생성자 함수를 이용해 bar 객체, baz 객체 생성
var bar = new Person("bar", 33, "woman");
console.dir(bar);

var baz = new Person("baz", 25, "woman");
console.dir(baz);
```

![constructor](https://user-images.githubusercontent.com/16531837/43558781-26ee9610-9646-11e8-8c46-0db83a79739b.png)

이렇게 차이가 발생하는 이유는 자바스크립트 객체 생성 규칙 때문이다. 자바스크립트 객체는 자신을 생성한 생성자 함수의 프로토타입 프로퍼티가 가리키는 객체를 자신의 프로토타입 객체로 설정한다.  
객체 리터럴 방식에서는 객체 생성자 함수가 Object()이며, 생성자 함수 방식의 경우는 생성자 함수 자체이다.

## refs

* 자바스크립트 웹 애플리케이션
* [자바스크립트 객체](https://poiemaweb.com/js-object)
* [생활코딩 - 생성자와 new](https://opentutorials.org/course/743/6570)
* [자바스크립트 생성자 함수와 프로토타입](https://doitnow-man.tistory.com/132)
