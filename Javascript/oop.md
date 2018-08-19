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

