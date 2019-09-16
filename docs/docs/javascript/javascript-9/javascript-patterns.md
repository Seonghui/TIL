---
layout: default
title: 자바스크립트 패턴
parent: 9. 기타
grand_parent: javascript
nav_order: 1
---

# 자바스크립트 패턴

1. Module Pattern
2. Prototype Pattern
3. Observer Pattern
4. Singleton Pattern

## 1. Module Pattern

모듈은 자바스크립트의 클래스와도 같다. 여기서 모듈은 즉시실행함수(IIFE)여야 한다. 클래스의 장점은 캡슐화인데, 모듈 패턴은 클로저를 사용해서 private members를 정의할 수 있다. 따라서 public 변수나 함수만 리턴이 되고, 나머지는 모두 클로저 안에서 private하게 위치하게 된다. 참고로 자바스크립트 자체에서는 private의 개념이 없다. 그저 함수의 스코프를 사용해서 흉내를 내는 것이다.

* 장점
  * 자바스크립트의 관점에서 캡슐화 구현이 가능하다.
  * public members가 private members에 접근 가능하지만 밖에서는 private members에 접근 불가능하다.
* 단점
  * public members와 private members의 접근방법이 달라서 뭔가를 바꾸고 싶을 때 public과 private 둘 다 변경해야 한다.
  * private members의 유닛테스트가 불가능하다.

### 1.1 Module Pattern 예시

```javascript
(function() {

    // declare private variables and/or functions

    return {
      // declare public variables and/or functions
    }

})();
```

같은 스코프에 있지 않는 이상 private 변수나 메소드에 접근할 수 있는 방법이 없다. 아래 예제의 경우, `callChangeHTML`는 반환된 객체를 바인드해 `HTMLChanger` 내부를 참조할 수 있다. 그런데 모듈 외부에서는 contents를 참조할 수 없다.

```javascript
var HTMLChanger = (function() { // 익명 모듈을 위한 namespace - HTMLChanger
  var contents = 'contents'

  var changeHTML = function() {
    var element = document.getElementById('attribute-to-change');
    element.innerHTML = contents;
  }

  return { // object 반환
    callChangeHTML: function() {
      changeHTML();
      console.log(contents);
    }
  };

})();

HTMLChanger.callChangeHTML(); // Outputs: 'contents' // 객체 리터럴 방식으로 호출
console.log(HTMLChanger.contents);  // undefined
```

### 1.2 Revealing Module Pattern

캡슐화를 유지하지만 모듈 패턴과 달리 명시적으로 노출하고 싶은 부분만 정해서 노출하는 방식.

* 장점
  * 깔끔함, 명시성이 좋음
  * 문법의 일관성
* 단점
  * 모듈 패턴과 같이 private 메소드에 접근할 방법이 없음(유닛 테스트 어려움)
  * private 메소드를 확장하기 어려움
  * private 메소드를 참조하는 public 메소드를 수정하기 어려움

```javascript
var Exposer = (function() {
  var privateVariable = 10;

  var privateMethod = function() {
    console.log('Inside a private method!');
    privateVariable++;
  }

  var methodToExpose = function() {
    console.log('This is a method I want to expose!');
  }

  var otherMethodIWantToExpose = function() {
    privateMethod();
  }

  return { // 노출하고 싶은 부분만 return
      first: methodToExpose,
      second: otherMethodIWantToExpose
  };
})();

Exposer.first();        // Output: This is a method I want to expose!
Exposer.second();       // Output: Inside a private method!
Exposer.methodToExpose; // undefined
```

## 2. Prototype Pattern

프로토타입 상속에 기반해서 만들어진 패턴이다. 쉽게 유지보수가 가능한 것이 장점이다.

### 2.1 prototype 객체를 사용해 메소드 추가하기

생성자 함수 TeslaModelS는 단일 TeslaModelS 객체(인스턴스)를 생성할 수 있다. TeslaModelS 객체는 생성자한테서 초기화된 상태를 유지한다. 덧붙여, 프로토타입을 사용해서 go 와 stop 함수를 쉽게 유지보수 할 수 있다.

```javascript
var TeslaModelS = function() {
  this.numWheels    = 4;
  this.manufacturer = 'Tesla';
  this.make         = 'Model S';
}

// 방법 1
TeslaModelS.prototype.go = function() {
  // Rotate wheels
}

TeslaModelS.prototype.stop = function() {
  // Apply brake pads
}

// 방법2
TeslaModelS.prototype = {
  go: function() { // Rotate wheels
  },
  stop: function() { // Apply brake pads
  }
}
```

### 2.2 Revealing Prototype Pattern

Revealing Module Pattern과 비슷하다. 객체를 반환하여 캡슐화를 한다. 객체를 반환하기 때문에 prototype 객체에 function 키워드를 선언한다.

```javascript
var TeslaModelS = function() {
  this.numWheels    = 4;
  this.manufacturer = 'Tesla';
  this.make         = 'Model S';
}

TeslaModelS.prototype = function() {

  // go와 stop 함수는 외부로부터 보호된다.
  var go = function() {
    // Rotate wheels
  };

  var stop = function() {
    // Apply brake pads
  };

  return {
    // 객체 반환
    pressBrakePedal: stop,
    pressGasPedal: go
  }
}();
```

## 3. Observer Design Pattern

애플리케이션의 한 부분이 변경될 때 다른 부분도 같이 변경되어야 하는 일이 있다. 만약 객체가 수정되면 그 객체와 연관된 객체들에게 변경되었다고 알려주는 것을 옵저버 패턴이라고 한다. 발행/구독 모델로 알려져 있기도 하다.

MVC 패턴도 예가 될 수 있다. 모델이 바뀌면 View도 업데이트 되기 떄문이다. MVC 아키텍쳐의 장점은 종속성을 줄이기 위해 뷰와 모델을 분리하는 것이다. 단, 단점은 observer 수가 많아지면 성능이 저하된다는 것이다.

### 3.1 자바스크립트로 Observer 만들기

```javascript
var Subject = function() {
  this.observers = [];
  var self = this;
  return {
    subscribeObserver: function(observer) {
      self.observers.push(observer);
    },
    unsubscribeObserver: function(observer) {
      var index = self.observers.indexOf(observer);
      if(index > -1) {
        self.observers.splice(index, 1);
      }
    },
    notifyObserver: function(observer) {
      var index = self.observers.indexOf(observer);
      if(index > -1) {
        self.observers[index].notify(index);
      }
    },
    notifyAllObservers: function() {
      for(var i = 0; i < self.observers.length; i++){
        self.observers[i].notify(i);
      }
    }
  };
};

var Observer = function() {
  return {
    notify: function(index) {
      var parsedIndex = Number(index) + 1
      console.log("Observer " + parsedIndex + " is notified!");
    }
  }
}

var subject = new Subject();

var observer1 = new Observer();
var observer2 = new Observer();
var observer3 = new Observer();
var observer4 = new Observer();

subject.subscribeObserver(observer1);
subject.subscribeObserver(observer2);
subject.subscribeObserver(observer3);
subject.subscribeObserver(observer4);
subject.unsubscribeObserver(observer3);

subject.notifyObserver(observer2); // Observer 2 is notified!

subject.notifyAllObservers();
// Observer 1 is notified!
// Observer 2 is notified!
// Observer 3 is notified!
```

## 4. Singleton Pattern

생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다. 이와 같은 디자인 유형을 싱글톤 패턴이라고 한다. 싱글톤 패턴은 클라이언트가 여러 객체를 생성하지 못하게 제한한다. 예를 들어 사무실에 10명의 사람이 있고, 그 사람들이 전부 하나의 프린터를 사용한다면, 열 개의 컴퓨터(인스턴스)는 하나의 프린터를 공유하는 것이 된다. 하나의 프린터를 사용해서 같은 자원을 공유하는 것이다.

### 4.1 getInstance 메소드를 사용해서 프린터 인스턴스를 생성하는 예제

```javascript
var printer = (function () {
  var printerInstance;

  function create () {
    function print() {
      // underlying printer mechanics
      console.log('print')
    }
    function turnOn() {
      // warm up
      // check for paper
      console.log('turnOn')
    }
    return {
      // public + private states and behaviors
      print: print,
      turnOn: turnOn
    };
  }

  return {
    // 만약 프린터 인스턴스가 존재하지 않는 경우에는 프린터 인스턴스를 생성하고
    // 존재하는 경우에는 만들어진 프린터 인스턴스를 반환한다
    getInstance: function() {
      if(!printerInstance) {
        printerInstance = create();
      }
      return printerInstance;
    }
  };

})();

var officePrinter = printer.getInstance();
officePrinter.print(); // print
officePrinter.turnOn(); // turnOn
```

## References

* [https://scotch.io/bar-talk/4-javascript-design-patterns-you-should-know](https://scotch.io/bar-talk/4-javascript-design-patterns-you-should-know)
* [wikipedia - 옵서버 패턴](https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%B4)
* [wikipedia - 싱글턴 패턴](https://ko.wikipedia.org/wiki/%EC%8B%B1%EA%B8%80%ED%84%B4_%ED%8C%A8%ED%84%B4)
