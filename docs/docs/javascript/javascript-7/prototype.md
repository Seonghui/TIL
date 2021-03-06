---
layout: default
title: 프로토타입
parent: 7. 객체지향 프로그래밍
grand_parent: javascript
nav_order: 2
---

# 프로토타입 (Prototype)

{: .no_toc }

{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 1. 프로토타입의 정의

* 자바스크립트는 프로토타입 기반 언어이다. 자바에서는 **상속**을 통해 코드를 재사용하는데, 자바스크립트는 **프로토타입**을 사용해서 코드 재사용을 한다. 프로토타입은 객체를 확장하고 객체 지향적인 프로그램을 할 수 있게 해준다.
* 자바스크립트는 클래스라는 개념이 없다. ECMAScript6 표준에는 Class 문법이 추가되었지만, 문법이 추가되었다는 거지 자바스크립트가 클래스 기반으로 바뀌었다는 말은 아니다. 클래스가 없으니 상속도 없는데, 프로토타입을 사용하면 상속을 흉내낼 수 있다.
* 프로토타입 기반 언어는 객체 원형인 프로토타입을 이용해서 새로운 객체를 만들어낸다. 이때 만들어진 객체 안에 Prototype Link 와 Prototype Object 라는 것이 존재하는데, 이것을 통틀어 프로토타입이라고 부른다.
* 이 프로토타입 링크를 사용자가 정의한 객체에 링크가 참조되도록 설정하면 객체 지향적인 프로그램이 가능해진다.

### 1.1 프로토타입의 종류

크게 2 가지로 해석된다.

#### 1.1.1 Prototype Object(프로토타입 객체, 프로토타입 프로퍼티)

프로토타입 객체는 새로운 객체가 생성되기 위한 원형이 되는 객체이다. 이 프로토타입 속성은 함수만 가지고 있는 속성으로, 함수가 생성될 때 만들어지며 같은 원형으로 생성된 객체가 공통으로 참조하는 공간이다. 예를 들어 모든 함수의 부모 객체는 Function.prototype 객체이다.  
참고로 Function.prototype 함수 객체도 결국 함수이다. 그렇다면 Function.prototype 의 부모는 뭘까? 바로 Object.prototype 이다.

#### 1.1.2 Prototype Link(**proto**속성)

객체 안의 **proto**속성은 자신을 만들어낸 원형인 프로토타입 객체를 참조하는 링크이다. 모든 객체가 빠짐없이 가지고 있는 속성으로, **proto**속성을 통해 상위 프로토타입과 연결되어있는 형태를 프로토타입 체인이라고 한다.  
모든 객체는 최상위에 있는 Prototype Object 를 가리키는데, 이러한 프로토타입 체인 구조 떼문에 객체는 Object 의 자식들이라고 부른다. 그리고 마치 객체지향의 상속처럼 부모 객체의 프로퍼티를 자신의 것처럼 사용할 수 있다. 대표적으로 Object.prototype 에 있는 프로퍼티인 toString 메소드가 있다.

#### 1.1.3 Prototype Object 와 Prototype Link 의 차이점

둘 다 프로토타입이라 불리는 건 동일하다. 하지만 둘의 차이점은 뭘까?

* Prototype Object
  * Prototype Object 는 자기 자신의 분신이며, 자신을 원형으로 만들어질 다른 객체가 참조할 프로토타입이 된다. 즉 새로운 객체들(하위)에게 물려줄 연결에 대한 속성이다.
* Prototype Link
  * 상위에서 물려받은 객체의 프로토타입에 대한 정보이다.

### References

* [Medium 포스트](https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67)
* [insanehong 님 블로그](http://insanehong.kr/post/javascript-prototype/)
* [Nextree 포스트](http://www.nextree.co.kr/p7323/)
