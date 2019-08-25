# 형변환

형변환은 3가지 타입으로만 가능하다.

* to string
* to boolean
* to number

## 1. 문자열로 형변환하기

String() 함수를 사용하면 명시적(explicit) 형변환 가능, 연산자(+) 사용하면 암시적(implicit) 형변환이 가능하다.

```js
String(123) // explicit
123 + ''    // implicit
```

원시 자료형은 string으로 형변환 가능. 대부분 변환하면 생각한대로 잘 동작한다.

```js
String(123)                   // '123'
String(-12.3)                 // '-12.3'
String(null)                  // 'null'
String(undefined)             // 'undefined'
String(true)                  // 'true'
String(false)                 // 'false'
```

symbol형은 명시적인 변환만 가능하다.

```js
String(Symbol('my symbol'))   // 'Symbol(my symbol)'
'' + Symbol('my symbol')      // TypeError is thrown
```

## 2. 논리형으로 형변환하기

Boolean() 함수로 명시적 형변환 가능, 이외에는 문맥이나 연산자( || && !)로 형변환이 가능하다. 심볼은 기본적으로 true값, 비어있는 객체나 배열도 true가 기본값이다.

```js
Boolean(2)          // explicit
if (2) { ... }      // implicit due to logical context
!!2                 // implicit due to logical operator
2 || 'hello'        // implicit due to logical operator
```

## 3. 숫자형으로 형변환하기

자바스크립트에서는 문자열을 숫자로 바꾸는 몇 가지 방법이 있다. Number 객체 생성자를 사용하면 명시적(explicit) 형변환 가능, 암시적 형변환은 너무 많으니까 레퍼런스를 참고하자.

### 3-1. Number 객체 생성자를 사용하는 방법

```js
const numStr = "33.3";
const num = Number(numStr);    // 33.3
```

### 3-2. 내장 함수인 parseInt나 parseFloat 함수를 사용하는 방법

Number 생성자와 비슷하게 동작하지만 몇 가지 다른 점이 있다. parseInt를 사용할 때는 기수를 넘길 수 있다. 기수는 변환할 문자열이 몇 진수 표현인지 지정한다. 그리고 parseInt와 parseFloat는 모두 숫자로 판단할 수 있는 부분까지만 변환하고, 그 뒤에 있는 문자열은 무시한다. 따라서 문자열의 상태가 엉망진창이어도 입력값으로 쓸 수 있다.

```js
const a = parseInt("16 volts", 10);   // volts는 무시된다. 10진수 16이다.
const b = parseInt("3a", 16);         // 16진수 3a를 10진수로 바꾼다. 결과는 58.
const c = parseFloat("15.5 kph");     // kph는 무시된다. parseFloat는 항상 기수가 10이라고 가정한다. 결과는 15.5
```

Date 객체를 숫자로 바꿀 때는 valueOf() 메서드를 사용한다. 이 숫자는 UTC 1970년 1월 1일 자정으로부터 몇 밀리초가 지났는지 나타내는 숫자이다.

```js
const d = new Date(); // 현재 날짜
const ts = d.valueOf(); // UTC 1970년 1월 1일 자정으로부터 몇 밀리초가 지났는지 나타내는 숫자
```

## refs

* [https://medium.freecodecamp.org/js-type-coercion-explained-27ba3d9a2839](https://medium.freecodecamp.org/js-type-coercion-explained-27ba3d9a2839)
* 러닝 자바스크립트
