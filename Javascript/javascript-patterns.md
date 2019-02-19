자바스크립트 패턴
---
* Module
* Prototype
* Observer
* Singleton

# Module Design Pattern

* 모듈은 자바스크립트의 클래스와도 같다. 클래스의 장점은 캡슐화(states와 behaviors를 다른 클래스로부터 보호)인데, 모듈 패턴은 클로저를 사용해서 private하게 만들 수 있음.
* public 변수나 함수만 리턴이 되고, 나머지는 모두 클로저 안에서 private하게 위치
* 모듈은 즉시실행함수(IIFE)여야함.
* 참고로 자바스크립트 자체에서는 private의 개념이 없기 때문에, 그저 함수의 스코프를 사용해서 흉내를 내는 것
* 장점
  * 자바스크립트의 관점에서 캡슐화 구현 가능
  * public parts가 private parts에 접근 가능하지만 밖에서는 private parts에 접근 불가능
* 단점
  * public members와 private members의 접근방법이 달라서 뭔가를 바꾸고 싶을 때 public과 private 둘다 변경해야함(무슨말?)
  * private members 유닛테스트 불가능

```javascript
(function() {

    // declare private variables and/or functions

    return {
      // declare public variables and/or functions
    }

})();
```

* 같은 스코프에 있지 않는 이상 private 변수나 메소드에 접근할 수 있는 방법이 없음. 아래 예제의 경우, `callChangeHTML`는 반환된 객체를 바인드해 `HTMLChanger` namespace 내부를 참조할 수 있음. 그런데 모듈 외부에서는 contents를 참조할 수 없음.

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

## Revealing Module Pattern
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

# Prototype Design Pattern
* 프로토타입 상속에 기반해서 만들어짐
* 쉽게 유지보수가 가능

```javascript
var TeslaModelS = function() {
  this.numWheels    = 4;
  this.manufacturer = 'Tesla';
  this.make         = 'Model S';
}

TeslaModelS.prototype.go = function() {
  // Rotate wheels
}

TeslaModelS.prototype.stop = function() {
  // Apply brake pads
}
```

* TeslaModelS 객체로 메소드 유지보수하기
```javascript
var TeslaModelS = function() {
  this.numWheels    = 4;
  this.manufacturer = 'Tesla';
  this.make         = 'Model S';
}

TeslaModelS.prototype = {
  go: function() {
    // Rotate wheels
  },
  stop: function() {
    // Apply brake pads
  }
}
```

## Revealing Prototype Pattern
* Revealing Module Pattern과 비슷

```javascript
var TeslaModelS = function() {
  this.numWheels    = 4;
  this.manufacturer = 'Tesla';
  this.make         = 'Model S';
}

TeslaModelS.prototype = function() {

  var go = function() {
    // Rotate wheels
  };

  var stop = function() {
    // Apply brake pads
  };

  return {
    pressBrakePedal: stop,
    pressGasPedal: go
  }

}();
```

# Observer Design Pattern
* 애플리케이션이 한 부분이 변경될 때 다른 부분도 같이 변경되어야 하는 일이 있음. 앵귤러JS에서는 `$scope` 객체가 변경될때 이걸 다른 컴포넌트들에게 알리기 위해 이벤트를 트리거할 수 있음. observer 패턴은 이런 방식임. 만약 객체가 수정되면 dependent 객체들에게 변경되었다고 알려줌.
* MVC 패턴도 example이 될 수 있음. 모델이 바뀌면 view도 업데이트됨. mvc 아키텍쳐의 장점은 종속성을 줄이기 위해 뷰와 모델을 분리하는 것임.
* 단점
    * observer 수가 많아지면 성능이 저하됨

* 자바스크립트로 observer 만들기
```javascript
var Subject = function() {
  this.observers = [];

  return {
    subscribeObserver: function(observer) {
      this.observers.push(observer);
    },
    unsubscribeObserver: function(observer) {
      var index = this.observers.indexOf(observer);
      if(index > -1) {
        this.observers.splice(index, 1);
      }
    },
    notifyObserver: function(observer) {
      var index = this.observers.indexOf(observer);
      if(index > -1) {
        this.observers[index].notify(index);
      }
    },
    notifyAllObservers: function() {
      for(var i = 0; i < this.observers.length; i++){
        this.observers[i].notify(i);
      };
    }
  };
};

var Observer = function() {
  return {
    notify: function(index) {
      console.log("Observer " + index + " is notified!");
    }
  }
}

var subject = new Subject();

var observer1 = new Observer();
var observer2 = new Observer();
var observer3 = new Observer();
var observer4 = new Observer();

subject.subscribeObserver(observer1); // 옵저버 구독
subject.subscribeObserver(observer2);
subject.subscribeObserver(observer3);
subject.subscribeObserver(observer4);

subject.notifyObserver(observer2); // Observer 2 is notified!

subject.notifyAllObservers();
// Observer 1 is notified!
// Observer 2 is notified!
// Observer 3 is notified!
// Observer 4 is notified!
```

## PUBLISH / SUBSCRIBE
* 알림을 수신하려는 객체와 이벤트를 발생시키는 객체 사이에 있는 topic/event 채널을 사용함.
* 구독자가 필요로 하는 값을 사용자 정의 인자로 넘길수 있는 애플리케이션별 이벤트 정의가 가능함
* PUBLISH / SUBSCRIBE 패턴은 observer 패턴과 다르지만 많이들 같이 사용함
* 예를 들어(앵귤러JS), subscriber는 `$on('event', callback)`로 이벤트를 구독하고, publisher는 `$emit('event', args)`이나 `$broadcast('event', args)`로 이벤트를 발행함.

# Singleton
* 단일 인스턴스 생성만 허용하지만, 이 인스턴스들은 같은 객체임.
* Singleton은 클라이언트가 여러 객체를 생성하지 못하게 제한하고, 첫 번째 객체가 생성되면 자기 자신을 반환함
* 예를 들어 사무실에 10명의 사람이 있고, 그 사람들이 전부 하나의 프린터를 사용한다면, 열 개의 컴퓨터(인스턴스)는 하나의 프린터를 공유하는 것이 된다는 말임. 하나의 프린터를 사용해서 같은 자원을 공유함.
* 아래는 getInstance 메소드를 사용해서 프린터 인스턴스를 생성하는 예제

```javascript
var printer = (function () {

  var printerInstance;

  function create () {

    function print() {
      // underlying printer mechanics
    }

    function turnOn() {
      // warm up
      // check for paper
    }

    return {
      // public + private states and behaviors
      print: print,
      turnOn: turnOn
    };
  }

  return {
    getInstance: function() {
      if(!printerInstance) {
        printerInstance = create();
      }
      return printerInstance;
    }
  };

  function Singleton () {
    if(!printerInstance) {
      printerInstance = intialize();
    }
  };

})();

var officePrinter = printer.getInstance();
```

# Refs
* https://scotch.io/bar-talk/4-javascript-design-patterns-you-should-know