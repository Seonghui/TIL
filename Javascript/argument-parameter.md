인자와 매개변수
===

# 1. 인자와 매개변수의 개념

* 인자: 어떤 함수를 호출시에 전달되는 값
* 매개변수: 그 전달된 인자를 받아들이는 함수

## 1.1. 예제

여기서 인자는 add() 함수를 호출할 때 전달되는 3,4 이고
매개변수는 add()함수 구현부의 x,y 이다.

```javascript
function add(x, y) {
  return x + y;
}

add(3, 4);
```

# 2. arguments 객체
arguments 객체는 함수에 전달된 인수에 해당하는 array 형태의 객체이다.

## 2.1 예제
```js
function func1(a, b, c) {
  console.log(arguments[0]);
  // expected output: 1

  console.log(arguments[1]);
  // expected output: 2

  console.log(arguments[2]);
  // expected output: 3
}

func1(1, 2, 3);
```

## 2.2 예제
```js
var foo = function(data) {
  console.log(arguments);
}

foo(1, 2, 3);
```

# refs
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/arguments