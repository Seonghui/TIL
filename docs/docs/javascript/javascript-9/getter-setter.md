---
layout: default
title: 자바스크립트의 getter와 setter
parent: 9. 기타
grand_parent: javascript
nav_order: 1
---

# 자바스크립트의 getter와 setter

```js
const person = {
  firstName: 'Mosh',
  lastName: 'Hamedani',
  get fullName() {
    return `${this.firstName} ${this.lastName}`
  },
  set fullName(value) {
    const parts = value.split(' ');
    this.firstName = parts[0];
    this.lastName = parts[1];
  }
}

person.fullName = 'John Smith';
console.log(person)
```

## getter

get 구문은 객체의 프로퍼티를 그 프로퍼티를 가져올 때 호출되는 함수로 바인딩한다. 어떤 프로퍼티에 접근할 때마다 그 값을 계산하도록 해야 하거나, 내부 변수의 상태를 명시적인 함수 호출 없이 보여주고 싶을 때, JavaScript의 getter를 이용할 수 있다. getter가 바인딩된 프로퍼티는 동시에 실제 값을 가질 수는 없지만, getter와 setter를 동시에 바인딩해 일종의 유사 프로퍼티(pseudo-property)를 만들 수는 있다.

### getter의 구문

```text
get prop() { ... }
get [expression]() { ... }
```

* prop: 주어진 함수를 바인딩할 프로퍼티의 이름이다.
* expression: ECMAScript 2015가 도입되면서, 함수의 이름을 계산하기 위해 표현식을 사용할 수 있다.

latest에 어떤 값을 할당하려고 시도해도 **그 값이 바뀌지 않는다**는 점을 유의해야 한다.

```js
var obj = {
  log: ['example','test'],
  get latest() {
    if (this.log.length == 0) return undefined;
    return this.log[this.log.length - 1];
  }
}

obj.latest = 'hello'
console.log(obj.latest); // "test".
```

## setter

setter는 어떤 객체의 속성에 이 속성을 설정하려고 할 때 호출되는 함수를 바인드한다. setter는 특정한 속성에 값이 변경되어 질 때마다 함수를 실행하는데 사용될 수 있다. Setter는 유사(pseudo)-property 타입을 생성하는 getter와 함께 가장 자주 사용된다.

### setter의 구문

```text
set prop(val) { ... }
set [expression](val) { ... }
```

* prop: 주어진 함수를 바인드할 속성의 이름
* val: prop에 설정될 값을 가지고 있는 변수의 별명.
* expression: ECMAScript 6부터는, 주어진 함수에 바인드 되는 속성 이름은 계산(computed)을 통해 얻을 수 있고 이것을 위한 expressions을 사용할 수 있다.

## References

* [MDN - getter](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/get)
