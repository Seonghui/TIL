# 생성자 함수

## 1. 생성자 함수의 개념

생성자 함수는 말 그대로 자바스크립트의 객체를 생성하는 역할을 한다. 하지만 여느 언어와는 달리 그 형식이 정해져 있지 않고, 기존 함수를 new 로 호출하면 해당 함수는 생성자 함수로 동작한다.

```javascript
function Person(name) {
  this.name = name;
}
var stella = new Person("Stella");
console.log(stella);
```

이는 반대로 생각하면 일반 함수에 new 를 붙여 호출하면 원치 않은 생성자 함수처럼 동작할 수 있다. 따라서 대부분의 자바스크립트 스타일 가이드에서는 생성자 함수의 함수 이름의 첫 문자를 대문자로 쓰기를 권하고 있다. 되도록이면 생성자 함수 이름은 대문자로 쓰자.

* 생성자 함수는 명시적으로 객체를 생성하지 않는다.
* 생성자 함수는 return 문이 없다.

### new 키워드

생성자를 호출할 때에는 항상 new 키워드를 이용해야 한다. new 키워드를 이용해 생성자 함수를 호출하면 컨텍스트가 전역\(윈도우\)에서 만들려고 하는 인스턴스 전용의 새로운 빈 컨텍스트로 바뀐다.

## 2. 생성자 함수가 동작하는 방식

new 연산자로 자바스크립트 함수를 생성자로 호출하면, 다음과 같은 순서로 동작한다.

1. 새 객체를 생성
2. 생성자의 this 값에 새 객체를 할당
3. 새 객체의 프로토타입 링크는 자신을 생성한 함수의 프로토타입 오브젝트를 참조
4. 생성자 함수 내부 코드 실행
5. this 로 바인딩된 새 객체를 반환

![constructor](https://user-images.githubusercontent.com/16531837/43557706-f2da1624-9640-11e8-8727-cd9ea75cf672.png)

## 3. 객체 리터럴 방식 vs 생성자 함수 방식

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
객체 리터럴 방식에서는 객체 생성자 함수가 Object\(\)이며, 생성자 함수 방식의 경우는 생성자 함수 자체이다.

## 4. 생성자 함수 new 를 붙이지 않고 호출할 경우

일반 함수 호출과 생성자 함수를 호출할 때 this 바인딩 방식이 다르기 때문에 오류가 발생할 수 있다

* 일반 함수 호출시: this 가 window 전역 객체에 바인딩됨.
* 생성자 함수 호출시: this 가 새로 생성되는 빈 객체에 바인딩됨.

```javascript
//생성자 함수
function Person(name, age, gender, position) {
  this.name = name;
  this.age = age;
  this.gender = gender;
}

var qux = Person("qux", 20, "man");
console.log(qux); // undefined

console.log(window.name); // qux
console.log(window.age); // 20
console.log(window.gender); // man
```

## refs

* 자바스크립트 웹 애플리케이션

