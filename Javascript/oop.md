객체지향 프로그래밍
===
자바스크립트는 프로토타입 기반의 언어이다. 클래스 기반 언어는 정확성, 안정성, 예측성 등의 관점에서 프로토타입 기반 언어보다 좀 더 나은 결과를 보장하지만, 프로토타입 기반 언어는 동적으로 자유롭게 객체의 구조와 동작 방식을 바꿀 수 있다는 장점이 있다.  
객체 지향 언어의 특성은 다음과 같다.
* 클래스, 생성자, 메서드
* 상속
* 캡슐화

# 1. 클래스, 생성자, 메서드
```javascript
function Person(arg) {
	this.name = arg;

	this.getName = function() {
		return this.name;
	}

	this.setName = function(value) {
		this.name = value;
	}
}

var me = new Person("zzoon");
console.log(me.getName());

me.setName("iamhjoo");
console.log(me.getName());
```
여기서 함수 Person은 클래스이자 생성자의 역할을 한다. 자바스크립트에서 클래스 기반의 객체 지향 프로그램은 기본적인 형태가 이와 같다.  
하지만 이 예제는 문제가 많다. 왜냐면 객체가 공통적으로 사용하는 setName() 함수와 getName()을 함수를 따로따로 생성하고 있기 때문이다. 이는 불필요하게 중복되는 영역을 메모리에 올려 자원을 낭비하는 것이 된다.

```javascript
function Person(arg) {
	this.name = arg;
}

Person.prototype.getName = function() {
	return this.name;
}

Person.prototype.setName = function(value) {
	this.name = value;
}

var me = new Person("me");
var you = new Person("you");
console.log(me.getName());
console.log(you.getName());
```
따라서 이렇게 프로토타입 객체에 정의해서 사용하는 것이 좋다.

```javascript
// 더글라스 크락포드가 제시한 메서드 정의 방법
Function.prototype.method = function(name, func) {
    this.prototype[name] = func;
}

function Person(arg) {
    this.name = arg;
}

Person.method("getName", function() {
    return this.name;
});

Person.method("setName", function(value) {
    this.name = value;
});

var me = new Person("me");
var you = new Person("you");
console.log(me.getName());
console.log(you.getName());
```
이렇게도 사용 가능하다.

# 2. 상속
자바스크립트는 객체 프로토타입 체인을 사용해서 상속을 구현할 수 있다. 상속 구현 방식은 크게 두 가지로 구분할 수 있다.
* 클래스 개념 없이 프로토타입으로 상속을 구현하는 방식
* 클래스 기반 전통적인 상속 방식을 흉내내는 것

## 2.1 프로토타입을 이용한 상속
### 2.1.1 create_object()
```javascript
function create_object(o) {
	function F() {}
	F.prototype = o;
	return new F();
}
```
![pi](https://user-images.githubusercontent.com/16531837/44308758-5e0d1c80-a3f6-11e8-8fe7-98cf81c4b2e7.png)
create_object() 함수는 인자로 들어온 객체를 부모로 하는 자식 객체를 생성하여 반환한다. 그림을 보면 새로운 빈 함수 객체 F를 만들고 F.prototype에 인자로 들어온 객체를 참조한다. 그리고 함수 객체 F를 생성자로 하는 새로운 객체를 만들어 반환한다.  
이렇게 프로토타입의 특성을 활용하여 상속을 구현하는 것이 프로토타입 기반의 상속이다. (참고로 create_object() 함수는 ES5에서 Object.create() 함수로 제공된다. 여기서는 프로토타입 기반 상속의 이해를 돕고자 사용한 것이다.)
### 2.1.2 create_object() 함수로 상속 구현하기
부모 객체에 해당하는 person 객체와 이 객체를 프로토타입 체인으로 참조할 수 있는 자식 객체 student를 만들어서 사용하였다.
```javascript
var person = {
	name : "zzoon",
	getName : function() {
		return this.name;
	},
	setName : function(arg) {
		this.name = arg;
	}
};

function create_object(o) {
	function F() {};
	F.prototype = o;
	return new F();
}

var student = create_object(person);

student.setName("me");
console.log(student.getName());
```
![pi2](https://user-images.githubusercontent.com/16531837/44308856-1dae9e00-a3f8-11e8-8b55-ed1c1bea368d.png)
### 2.1.3 extend() 함수로 메서드 추가하기
얕은 복사를 사용하는 extend() 함수를 사용해서 stduent 객체를 확장시켰다.
```javascript
var person = {
	name : "zzoon",
	getName : function() {
		return this.name;
	},
	setName : function(arg) {
		this.name = arg;
	}
};

function create_object(o) {
	function F() {};
	F.prototype = o;
	return new F();
}

function extend(obj,prop) {
	if ( !prop ) { prop = obj; obj = this; }
	for ( var i in prop ) obj[i] = prop[i];
	return obj;
};

var student = create_object(person);

student.setName("me");
console.log(student.getName());

var added = {
	setAge : function(age) {
		this.age = age;
	},
	getAge : function() {
		return this.age;
	}
};

extend(student, added);

student.setAge(25);
console.log(student.getAge());

```
![pi3](https://user-images.githubusercontent.com/16531837/44308990-3455f480-a3fa-11e8-9702-a50465195178.png)

## 2.2 클래스 기반의 상속
클래스 기반의 상속이라고 하지만, 함수의 프로토타입을 적절하게 섞어서 상속을 구현해낸다. 다른 점이라고는 클래스의 역할을 하는 함수로 상속을 구현한다는 것이다.
```javascript
function Person(arg) {
    this.name = arg;
}

Function.prototype.method = function(name, func) {
    this.prototype[name] = func;
}

Person.method("setName", function(value) {
    this.name = value;
});
Person.method("getName", function() {
    return this.name;
});

function Student(arg) {
}

function F() {};
F.prototype =Person.prototype;
Student.prototype = new F();
Student.prototype.constructor = Student;
Student.super = Person.prototype;

var me = new Student();
me.setName("zzoon");
console.log(me.getName());
```
빈 함수 F()를 생성하고 이 F()의 인스턴스를 Person.prototype과 Student 사이에 두었다. 그리고 이 인스턴스를 Student.prototype에 참조되게 한다.  
즉 빈 함수(F)의 객체를 중간에 두어 Person의 인스턴스와 Student 의 인스턴스를 서로 독립적으로 만들었다. 이제 Person 함수 객체에서 this에 바인딩되는 것은 Studentd의 인스턴스가 접근할 수 없다.