---
layout: default
title: 객체지향 프로그래밍 (OOP)
parent: 7. 객체지향 프로그래밍
grand_parent: javascript
nav_order: 1
---

# 객체지향 프로그래밍 \(OOP\)

자바스크립트는 프로토타입 기반의 언어이다. 클래스 기반 언어는 정확성, 안정성, 예측성 등의 관점에서 프로토타입 기반 언어보다 좀 더 나은 결과를 보장하지만, 프로토타입 기반 언어는 동적으로 자유롭게 객체의 구조와 동작 방식을 바꿀 수 있다는 장점이 있다.  
객체 지향 언어의 특성은 다음과 같다.

* 클래스, 생성자, 메소드
* 상속
* 캡슐화

## 1. 기본 용어

* 객체: 이름이 지정된 프로퍼티들의 모음. key, value 쌍으로 되어있으며 순서가 보장되지 않음\(순서가 없음\).
* 클래스: **어떤 자동차**처럼 추상적이고 범용적인 것
* 인스턴스: **마티즈**처럼 구체적이고 한정적인 것. 인스턴스를 처음 만들 때는 생성자가 실행된다. 이때 생성자는 객체 인스턴스를 초기화한다.
* 메소드: 기능
* 클래스 메소드: 클래스에 속하지만 특정 인스턴스에 묶이지는 않는 메소드 \(ex. 시동을 거는 기능\)
* 생성자\(constructor\): 인스턴스를 생성하고 클래스 필드를 초기화하기 위한 특수한 메소드. 클래스 필드는 클래스 내부의 캡슐화된 변수를 말한다. 데이터 멤버 또는 멤버 변수라고도 부른다. 쉽게 말해 자바스크립트의 생성자 함수에서 this에 추가한 프로퍼티를 클래스 기반 객체지향 언어에서는 클래스 필드라고 부른다.

### 1.1 클래스의 개념

OOP는 클래스를 계층적으로 분류하는 수단이 될 수 있다. 예를 들어 **자동차**보다 더 범용적인 **운송 수단**이라는 클래스가 있다. 만약 운송 수단이라는 클래스에 자동차가 속한다면, 운송 수단은 자동차의 **슈퍼 클래스**가 되고, 자동차는 운송 수단의 **서브 클래스**가 된다.  
운송 수단 클래스에는 자동차, 보트, 비행기 등 여러가지 서브클래스가 있을 수 있고, 서브클래스 역시 서브클래스를 가질 수 있다. 예를 들면 보트 자동차 서브클래스에는 트럭, 해치백, 세단, 경차 등의 서브클래스가 있을 수 있다.

### 1.2 클래스와 인스턴스

여기서 자동차, 경차 등 클래스는 추상적인 개념이지만, 그 클래스에 속하는 마티즈는 실제로 볼 수 있는 구체적인 대상이기 때문에 인스턴스라고 한다.

```js
var arr = [1, 2, 3]
```

arr라는 배열을 생성하면, 이 배열은 사실은 Array라는 생성자 함수로 new 연산자와 함께 호출한 결과물과 같다. Array 생성자 함수가 일반적인 개념의 클래스이다. 왜냐하면 Array 그 자체로는 특별한 일을 수행한다기 보다는 주로 new 연산자를 통해 생성한 배열 객체들의 기능을 정의하는 데에 주력하고 있기 때문이다. 배열에서 쓰이는 메소드들은 모두 Array.prototype에 정의되어 있다.

여기서 클래스를 통해 생성한 객체 [1,2,3]는 바로 인스턴스라고 한다. 구체적인 데이터를 지니고, 실제 코드상에 동작을 수행하는 실체이다.

### 1.3 알아두면 좋은 것

* 클래스의 인스턴스는 모두 같은 프로토타입을 공유한다
* 프로토타입에 프로퍼티나 메소드가 있다면 해당 클래스의 인스턴스는 모두 그 프로퍼티나 메소드에 접근할 수 있다
* 프로퍼티를 프로토타입에 정의하지 말고 인스턴스에 정의하는게 좋음

## 2. ES5의 클래스

### 1. 클래스, 생성자, 메소드

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
하지만 이 예제는 문제가 많다. 왜냐면 객체가 공통적으로 사용하는 setName\(\) 함수와 getName\(\)을 함수를 따로따로 생성하고 있기 때문이다. 이는 불필요하게 중복되는 영역을 메모리에 올려 자원을 낭비하는 것이 된다.

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

new 키워드로 만든 새 객체는 생성자의 prototype 프로퍼티에 접근할 수 있다.  
따라서 이렇게 프로토타입 객체에 정의해서 사용하는 것이 좋다.

```javascript
// 더글라스 크락포드가 제시한 메소드 정의 방법
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

### 2. 상속

자바스크립트는 객체 프로토타입 체인을 사용해서 상속을 구현할 수 있다. 상속 구현 방식은 크게 두 가지로 구분할 수 있다.

* 클래스 개념 없이 프로토타입으로 상속을 구현하는 방식
* 클래스 기반 전통적인 상속 방식을 흉내내는 것

#### 2.1 프로토타입을 이용한 상속

**2.1.1 create\_object\(\)**

```javascript
function create_object(o) {
    function F() {}
    F.prototype = o;
    return new F();
}
```

 

![pi](https://user-images.githubusercontent.com/16531837/44308758-5e0d1c80-a3f6-11e8-8fe7-98cf81c4b2e7.png)

create\_object\(\) 함수는 인자로 들어온 객체를 부모로 하는 자식 객체를 생성하여 반환한다. 그림을 보면 새로운 빈 함수 객체 F를 만들고 F.prototype에 인자로 들어온 객체를 참조한다. 그리고 함수 객체 F를 생성자로 하는 새로운 객체를 만들어 반환한다.  
이렇게 프로토타입의 특성을 활용하여 상속을 구현하는 것이 프로토타입 기반의 상속이다. \(참고로 create\_object\(\) 함수는 ES5에서 Object.create\(\) 함수로 제공된다. 여기서는 프로토타입 기반 상속의 이해를 돕고자 사용한 것이다.\)

**2.1.2 create\_object\(\) 함수로 상속 구현하기**

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

**2.1.3 extend\(\) 함수로 메소드 추가하기**

얕은 복사를 사용하는 extend\(\) 함수를 사용해서 stduent 객체를 확장시켰다.

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

#### 2.2 클래스 기반의 상속

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

빈 함수 F\(\)를 생성하고 이 F\(\)의 인스턴스를 Person.prototype과 Student 사이에 두었다. 그리고 이 인스턴스를 Student.prototype에 참조되게 한다.  
즉 빈 함수\(F\)의 객체를 중간에 두어 Person의 인스턴스와 Student 의 인스턴스를 서로 독립적으로 만들었다. 이제 Person 함수 객체에서 this에 바인딩되는 것은 Studentd의 인스턴스가 접근할 수 없다.

### 3. 캡슐화

캡슐화로 정보 은닉을 할 수 있다. 자바의 경우 public이나 private 멤버를 선언함으로써 해당 정보를 외부로 노출시킬지 여부를 결정하지만 자바스크립트는 이러한 키워드 자체를 지원하지 않는다.

```javascript
var Person = function(arg) { 
    var name = arg ? arg : "zzoon" ;

    var Func = function() {}
    Func.prototype = {
        getName : function() {
            return name;
        },
        setName : function(arg) {
            name = arg;
        }
    };

    return Func;
}();


var me = new Person();
console.log(me.getName());
```

클로저를 사용해서 name에 접근할 수 없다.

## ES6의 클래스

* ES6의 클래스는 함수이다. 모양만 클래스처럼 보일 뿐 typeof로 타입체크를 하면 function이 나온다.
* 그래서 여전히 클래스 인스턴스에서 사용할 수 있는 메소드라고 하면 프로토타입 메소드를 가리킨다. 실제로 콘솔에 Car.prototype을 찍어보면 메소드와 생성자가 찍힌다.

### 1. 클래스 생성하고 인스턴스 만들기

```javascript
// 클래스 생성
// 여기서의 this는 메소드를 호출한 인스턴스를 가리킨다
class Car {
    constructor(make, model) { //생성자
        this.make = make;
        this.model = model;
        this.userGears = ['P', 'N', 'R', 'D'];
        this.userGear = this.userGears[0];
    }

    // 기어변속 메소드 생성 
    shift(gear) {
        if (this.userGears.indexOf(gear) < 0)
            throw new Error(`Invalid gear: ${gear}`);
        this.userGear = gear;
    }
}

// 인스턴스 만들기
const car = new Car('Tesla', 'Model S');
car.shift('D');
car.userGear; // 'D'
```

Car 클래스에 shift 메소드를 사용하면 잘못된 기어를 선택하는 실수를 방지할 수 있을 것처럼 보이지만 완벽하게 보호되는 것은 아님. 직접 `car.userGear = 'X'`라고 설정하면 막을 수 없음. 따라서 가짜 접근 제한이나 WeakMap 인스턴스를 사용해야 함. 가짜 접근 제한은 단순 명시일 뿐이고, WeakMap을 사용해야 진정한 제한을 할 수 있음.

### 2. 가짜 접근 제한

그냥 단순하게 말해서 외부에서 접근하면 안 되는 프로퍼티 이름 앞에 밑줄을 붙이는 것임. 진정한 제한 아님.

```javascript
class Car {
    constructor(make, model) {
        this.make = make;
        this.model = model;
        this._userGears = ['P', 'N', 'R', 'D'];
        this._userGear = this._userGears[0];
    }

    // 기어변속 메소드 생성 
    shift(gear) {
        if (this._userGears.indexOf(gear) < 0)
            throw new Error(`Invalid gear: ${gear}`);
        this._userGear = gear;
    }
}

// 인스턴스 만들기
const car = new Car('Tesla', 'Model S');
car.shift('D');
car._userGear; // 'D'
```

### 3. 상속

```javascript
class Vehicle {
    constructor() {
        this.passengers = [];
        console.log('Vehicle created');
    }
    addPassenger(p) {
        this.passengers.push(p)
    }
}

class Car extends Vehicle {
    constructor() {
        super();
        console.log('car created');
    }
    deployAirbags() {
        console.log('bwoosh!!');
    }
}

const v = new Vehicle();
v.addPassenger("Frank");
v.addPassenger("Judy");

console.log(v.passengers); // ["Frank", "Judy"]

const c = new Car();
c.addPassenger("Alice");
c.addPassenger("Cameron");

console.log(c.passengers); // ["Alice", "Cameron"]

v.deployAirbags(); // Uncaught TypeError: v.deployAirbags is not a function
c.deployAirbags();
```

#### extends

* 어떤 클래스의 서브클래스로 만들어줌

#### super\(\)

* 슈퍼클래스의 생성자를 호출하는 특별한 함수
* 서브클래스에서는 이 함수르 반드시 호출해야 한다. 호출하지 않으면 에러가 발생함.

### 4. 다형성

* 슈퍼클래스의 멤버인 인스턴스를 나타냄
* 자바\(스크립트가 아닌 자바\)에서는 하나의 메소드나 클래스가 있을 때 이것들이 다양한 방법으로 동작하는 것을 의미.
* 키보드를 예를 들면, 키보드의 키를 사용하는 것은 '누른다' 이다. 하지만 똑같은 동작 방법의 키라고 하더라도 ESC는 취소를, ENTER는 실행의 목적을 가지고 있다. 다형성이란 동일한 조작방법으로 동작시키지만 동작방법은 다른 것을 의미한다.

## Ref

* 러닝 자바스크립트
* 인사이드 자바스크립트
* [https://poiemaweb.com/es6-class](https://poiemaweb.com/es6-class)
