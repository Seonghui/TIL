---
layout: default
title: 자바스크립트의 getter와 setter
parent: 9. 기타
grand_parent: javascript
nav_order: 1
---

# 자바스크립트의 getter와 setter

```js
var objA = {
  x: 10
};

var objB = objA;
objB.x = 1000;

console.log(objA.x) // 1000
console.log(objB.x) // 1000
```

objB.x를 변경하면 objA.x의 값도 같이 변경된다. objB가 objA를 참조하기 때문이다.

```javascript
var objA = {
  x: 10
};

var value = objA.x;
value = 1000;

console.log(objA.x);
console.log(value)
```

하지만 이렇게 객체의 값을 지정해서 넣어주면, 변수 value는 obj.x를 참조하는 대신 값복사를 하게 된다. 따라서 objA의 x값은 유지되고 value값만 변경된다.