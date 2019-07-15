# 형변환

형변환은 3가지 타입으로만 가능

* to string
* to boolean
* to number

### 1. 문자열로 형변환하기

* String\(\) 함수를 사용하면 명시적\(explicit\) 형변환 가능. 
* 연산자\(+\) 사용하면 암시적\(implicit\) 형변환 가능

  ```text
  String(123) // explicit
  123 + ''    // implicit
  ```

* 원시 자료형은 string으로 형변환 가능. 대부분 변환하면 생각한대로 잘 동작한다.

  ```text
  String(123)                   // '123'
  String(-12.3)                 // '-12.3'
  String(null)                  // 'null'
  String(undefined)             // 'undefined'
  String(true)                  // 'true'
  String(false)                 // 'false'
  ```

* symbol형은 명시적인 변환만 가능

  ```text
  String(Symbol('my symbol'))   // 'Symbol(my symbol)'
  '' + Symbol('my symbol')      // TypeError is thrown
  ```

### 2. 논리형으로 형변환하기

* Boolean\(\) 함수로 명시적 형변환 가능
* 이외에는 문맥이나 연산자\( \|\| && !\)로 형변환

  ```text
  Boolean(2)          // explicit
  if (2) { ... }      // implicit due to logical context
  !!2                 // implicit due to logical operator
  2 || 'hello'        // implicit due to logical operator
  ```

* false가 출력되는 값들
* 심볼은 기본적으로 true값, 비어있는 객체나 배열도 true

### 3. 숫자형으로 형변환하기

* Number\(\) 함수를 사용하면 명시적\(explicit\) 형변환 가능.
* 암시적 형변환은 너무 많음\(원문 참고\). 아래는 암시적 형변환의 각종 예시

## refs

* [https://medium.freecodecamp.org/js-type-coercion-explained-27ba3d9a2839](https://medium.freecodecamp.org/js-type-coercion-explained-27ba3d9a2839)

