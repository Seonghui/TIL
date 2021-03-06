---
layout: default
title: this 바인딩
parent: 5. 함수
grand_parent: javascript
nav_order: 4
---

# this

## this 바인딩

자바스크립트의 this 는 함수 호출 패턴에 따라 this 가 어떤 객체의 this 가 될 지 정해진다.\(어디에 바인딩될지 정해짐\) 이를 this 바인딩이라고 한다.

## 1. this 바인딩의 호출 패턴

* **객체의 메서드** 호출할 때 this 바인딩
* **함수**를 호출할 때 this 바인딩
* **생성자 함수**를 호출할 때 this 바인딩

### 1.1 객체의 메서드 호출할 때 this 바인딩

자신을 호출한 객체에 바인딩이 된다.

```javascript
var obj = {
  sayName: function() {
    console.log(this.name);
  }
};

var kim = obj;
kim.name = "kim";
kim.sayName(); // "kim"

var lee = obj;
lee.name = "lee";
lee.sayName(); // "lee"
```

### 1.2 함수를 호출할 때 this 바인딩

전역 객체\(window\)에 바인딩이 된다.

```javascript
var val = "Hello";

var func = function() {
  console.log(this.val);
};
func(); // "Hello"
console.log(this.val); // "Hello"
console.log(window.val); //  "Hello"
console.log(this === window); // true
```

하지만 함수 호출에서의 this 바인딩은 내부 함수를 호출할 때도 그대로 적용이 된다. 아래 예제를 보면, 결과가 2,3,4 로 나와야 할 것 같지만 그렇게 나오지 않는다. 왜냐면 자바스크립트가 내부 함수 호출 패턴을 정해놓지 않았기 때문에, 내부 함수도 함수 취급을 받아 이러한 결과가 나온 것이다.  
따라서 함수 호출 패턴 패턴 규칙에 따라 내부 함수의 this 는 전역 객체\(window\)에 바인딩된다.

```javascript
// 전역 변수 value 정의
var value = 100;

// myObject 객체 생성
var myObject = {
  value: 1,
  func1: function() {
    this.value += 1;
    console.log("func1() called. this.value : " + this.value);

    // func2() 내부 함수
    func2 = function() {
      this.value += 1;
      console.log("func2() called. this.value : " + this.value);

      // func3() 내부 함수
      func3 = function() {
        this.value += 1;
        console.log("func3() called. this.value : " + this.value);
      };

      func3(); // func3() 내부 함수 호출
    };
    func2(); // func2() 내부 함수 호출
  }
};
myObject.func1(); // func1() 메서드 호출
```

위와 같은 상황을 피하려면, 부모 함수의 this 를 내부 함수가 접근 가능한 다른 변수에다 저장해야 한다. 보통 this 값을 저장하는 변수의 이름을 that 이라고 짓는다.

```javascript
// 내부 함수 this 바인딩
var value = 100;

var myObject = {
  value: 1,
  func1: function() {
    var that = this; // 부모 함수의 this를 that으로 저장

    this.value += 1;
    console.log("func1() called. this.value : " + this.value);

    func2 = function() {
      that.value += 1;
      console.log("func2() called. this.value : " + that.value);

      func3 = function() {
        that.value += 1;
        console.log("func3() called. this.value : " + that.value);
      };
      func3();
    };
    func2();
  }
};

myObject.func1(); // func1 메서드 호출
```

### 1.3 생성자 함수를 호출할 때 this 바인딩

new 로 생성자 함수를 생성한 객체에 바인딩이 된다.

```javascript
var Person = function(name) {
  this.name = name;
};

var kim = new Person("kim");
console.log(kim.name); //kim

var lee = new Person("lee");
console.log(lee.name); //lee
```

