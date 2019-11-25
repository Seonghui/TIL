---
layout: default
title: 자료형
parent: 2. 변수, 상수, 데이터 타입
grand_parent: javascript
nav_order: 2
---

# 1. 자바스크립트의 자료형

{: .no_toc }
{: .no_toc .text-delta }

1. TOC
{:toc}

---

* 자료형을 선언하지 않아도 된다. 그래도 자료형은 존재한다.
* 동적 자료형(dynamically Typed)과 약타입(weakly type)이 존재한다.
* 약한 자료형 언어는 한번 정해진 자료형을 또다른 자료형으로 바꾸는 것이 가능하다.
* typeof 연산자로 데이터 타입 확인 가능하다.
* 객체가 아니고 메소드도 가지지 않는다.
* 기본 자료형은 생성자 혹은 부모 객체를 가진다.
* 자바스크립트의 값은 원시 값 혹은 객체이다.

## 1.1 동적 자료형과 정적 자료형

* 정적 자료형은 아닌데 타입스크립트 사용해서 정적 자료형처럼 사용 가능하다. 여기서 정적 자료형이란 자료형이 강제되고 쉽게 바꿀 수 없다는 것을 의미한다.
* 동적 자료형은 런타임 시점에 변수의 자료형을 추론한다. 일단 코드가 실행되면 컴파일러나 인터프리터는 **변수를 먼저 읽고 난 뒤 자료형을 결정**한다. 자료형이 강제되는 것은 같지만 자료형을 결정하는 시점이 다르다. 다시 말해 자료형 결정은 자바스크립트 엔진이 한다.

## 1.2 원시 타입도 참조 타입처럼 다룰 수 있다

다른 프로그래밍 언어는 원시 타입은 스택에 저장하고 참조는 힙에 저장하여 원시 타입과 참조 타입을 구분하고 있지만, 자바스크립트는 그렇지 않다.
자바스크립트에서는 **변수 객체(Variable object)** 의 스코프를 따라 변수를 추적한다. 원시 타입의 원시 값은 바로 변수 객체에 저장되지만 참조 타입에서 변수 객체에 저장되는 참조값은 메모리에 있는 실제 데이터를 가리키는 포인터이다.

# 2. 기본 자료형 (원시 타입)

단순한 데이터를 저장한다. 단 하나의 값만 나타낼 수 있고 불변이다.

* Number - 정수 또는 부동소수점 실수 값. (int, float 등...)
* String - characters의 배열 (예를 들면 단어)
* Boolean - true 또는 false
* Symbol - 다른 값들과 같지 않은 유니크한 값
* Null - null이라는 값만 있는 원시 타입. (값이 없는 상태)
* Undefined - undefined라는 값만 있는 타입 (변수가 선언되었지만 아직 값 할당이 안 된 상태.)

## 2-1. 숫자

3이나 5.5처럼 컴퓨터가 정확히 나타낼 수 있는 숫자도 있지만, 근사치로만 표현할 수 있는 숫자도 있다. 예를 들어 원주율은 절대 컴퓨터로 표현할 수 없다. 원주율을 구성하는 숫자는 무한하고 반복되지도 않기 때문이다.

자바스크립트도 다른 프로그래밍 언어와 마찬가지로 실제 숫자의 근사치를 저장할 때 IEEE-764배정도 부동소수점 숫자 형식을 사용한다. 이 형식을 더블이라고 부른다. 그런데 더블 형식의 근사치 결과는 종종 사람들을 당혹스럽게 만들곤 한다. 예를 들어 자바스크립트에서 **0.1 + 0.2는 0.30000000000000004를 반환**한다. 이 결과는 자바스크립트에 버그가 있거나 덧셈을 할 줄 몰라서가 아니다. 이건 무한한 값을 유한한 메모리 안에서 가능한 한 정확히 짐작하려다 생긴 결과일 뿐이다.

자바스크립트에는 숫자형 데이터 타입이 하나밖에 없는데, 이건 흔치 않은 일이다. 대부분의 프로그래밍 언어는 여러 가지 정수타입을 사용하며 부동소수점 숫자 타입도 두 가지 이상 사용한다. 숫자형 데이터를 하나만 갖기로 한 선택은 자바스크립트를 단순한 언어로, 특히 초보자에게 부담 없는 언어로 만들었다는 장점이 있다. 반면, 자바스크립트를 고성능 정수 연산이나 정밀한 소수점 연산이 필요한 애플리케이션에서 쓸 수 없게 만든 선택이기도 하다.

자바스크립트는 10진수, 2진수, 8진수, 16진수 네 가지 숫자형 리터럴을 인식한다. 10진 리터럴에는 소수점 없는 정수, 소수점 있는 10진수, 과학에서 사용하는 지수 표기법을 쓸 수 있다. 10진수, 16진수, 지수 등 어떤 리터럴 형식을 사용하더라도 결국 숫자는 더블 형식으로 저장된다. 그 외에도 무한대, 음의 무한대, '숫자가 아님'을 나타내는 특별한 값들이 있다. 이것들은 엄밀히 말해 숫자형 리터럴이 아니지만, 숫자형 값이므로 여기 포함시킨다.

## 2-2. 문자

자바스크립트 문자열은 유니코드 텍스트이다. 유니코드 자체는 모든 언어의 텍스트를 나타낼 수 있지만, 유니코드를 사용하는 소프트웨어가 모든 코드 포인트를 정확히 렌더링한다고 보장하지는 않는다.

```javascript
let count = 10;
const = blue = 0x0000ff;    // 16진수
const = umask = 0o0022;     //  8진수
const roomTemp = 21.5       // 10진수
const c = 3.0e6;            // 지수 (3.0 * 10^6 = 3,000,000)
const e = -1.6e-19          // 지수 (-1.6 * 10^-19 = 0.00000000000000000016)
const inf = Infinity;
const ninf = -Infinity;
const nam = NaN;
```

* escape

```javascript
const dialog1 = "He looked up and said \"don't do that!\" to Max.";
const dialog2 = 'He looked up and said "don\'t do that!" to Max.';
const s = "In JavaScript, use \\ as an escape character in strings.";
```

* template (**es6**)

```javascript
let currentTemp = 19.5;
const message = `The current temperature is ${currentTemp}\u00b0C`;
```

* multiline (**es6**)

```javascript
const multiline =
`
line1
line2
`
```

* String and Number

```javascript
const result1 = 3 + '30';    // '330'
const result2 = 3 * '30';    // 90
```

## 2-3. 불리언

true, false 두 가지 값밖에 없는 데이터 타입이다. 따옴표 안에 넣지 말자.

```javascript
let heating = true;
let cooling = false;
```

## 2-4. 심볼

유일한 토큰을 나타내기 위해 ES6에서 도입한 새 데이터 타입이다. 심볼은 항상 유일하다. 다른 어떤 심볼과도 일치하지 않는다. 이런 면에서 심볼은 객체와 유사하다. 객체는 모두 유일하다. 항상 유일하다는 점을 제외하면 심볼은 원시 값의 특징을 모두 가지고 있으므로 확장성 있는 코드를 만들 수 있다.

심볼은 `Symbol()` 생성자로 만든다.

```javascript
const RED = Symbol('The color of a sunset!');
const ORANGE = Symbol('The color of a sunset!');
RED !== ORANGE  // true; 심볼은 서로 다릅니다.
```

## 2-5. null과 undefined

`null`과 `undefined` 는 자바스크립트의 특별한 타입이다. null이 가질 수 있는 값은 null 하나뿐이며, undefined가 가질 수 있는 값도 undefined 하나뿐이다. null과 undefined는 모두 존재하지 않는 것을 나타낸다.

_null_ 프로그래머에게 허용된 데이터 타입이다. 만약 아직 변수의 값을 모르거나 적용할 수 없는 경우에는 undefined보다는 null을 사용하는 것이 더 나은 선택이다.

_undefined_ 자바스크립트 자체에서 사용한다. 변수를 선언하기만 하고 명시적으로 값을 할당하지 않으면 그 변수에는 기본적으로 undefined가 할당된다.

```javascript
let currentTemp;           // 임시적으로 undefined
const targetTemp = null;   // null, 즉 아직 모르는 값
currentTemp = 19.5;        // targetTemp에 19.5라는 값을 부여
currentTemp = undefined;   // undefined. 할당된 값에 다시 undefined를 쓰는 권장하지 않음. 이련 경우라면 null을 사용하길 권장
```

# 3. 객체 (참조 타입)

참조 타입은 메모리 상의 주소를 가리킨다. 원시 타입과 달리 여러 가지 값이나 복잡한 값을 나타낼 수 있으며, 변할 수도 있다.객체의 본질은 컨테이너이다. 컨테이너의 내용물은 시간이 지나면서 바뀔 수 있지만, 내용물이 바뀐다고 컨테이너가 바뀌는 건 아니다. 즉 여전히 같은 객체이다.

* Array
* Date
* RegExp
* Map과 WeakMap
* Set과 WeakSet
* Number, String, Boolean

## 3-1. 객체(Object) 자세히 알아보기

빈 객체는 아래와 같다.

```javascript
let obj = new Object();
// 혹은
let obj = {};
```

참조 타입은 할당된 변수에 값을 직접 저장하지 않으므로, 이 예제에서 obj 변수에 저장된 값은 사실 객체 인스턴스가 아니라 **객체가 있는 메모리상의 위치를 가리키는 포인터(또는 참조)**이다. 이것이 바로 객체와 원시 값의 가장 큰 차이점인데, **원시 값은 변수에 직접 값 자체가 저장**된다.



객체의 콘텐츠는 프로퍼티 또는 멤버라고 부른다. 프로퍼티는 이름(키)과 값으로 구성된다. 프로퍼티 접근은 멤버 접근 연산자 `.`, `[]`를 사용하면 된다.

```javascript
const sam3 = {
    name: 'Sam',
    classification: {
        kingdom: 'Anamalia',
        phylum: 'Chordata',
        class: 'Mamalia',
        family: 'Felidae',
    },
};

// 접근
sam3.classification.family;
sam3["classification"].family;
sam3.classification["family"];
sam3["classification"]["family"];

// 속성 제거
delete sam3.classification
```

## 3-2. Number, String, Boolean 객체

원시 타입인 숫자와 문자열, 불리언에는 각각 대응하는 객체 타입인 Number, String, Boolean이 있다. 이 객체에는 두 가지 목적이 있다. 하나는 Number.INFINITY 같은 특별한 값을 저장하는 것이고, 다른 하나는 함수 형태로 기능을 제공하는 것이다.

```javascript
const s = 'hello'
s.toUpperCase(); // HELLO
```

위 예제에서 s는 마치 객체처럼, 즉 함수 프로퍼티를 가진 것처럼 보인다. 하지만 s는 분명 원시 문자열 타입이다. 어떻게 된 걸까? 자바스크립트는 일시적인 String 객체를 만든다. 이 임시 객체에 toUpperCase 함수가 들어있다. 자바스크립트는 함수를 호출하는 즉시 임시 객체를 파괴한다. 객체가 임시로 만들어진다는 사실은 다음과 같이 문자열에 프로퍼티를 할당해 보면 알 수 있다.

```javascript
const s = 'hello';
s.rating = 3; // 에러가 없다
s.raiting; // 하지만 undefined
```

이 예제는 마치 문자열 s에 프로퍼티를 할당하는 것처럼 보인다. 사실은 일시적인 String 객체에 프로퍼티를 할당한 것이다. 임시 객체는 즉시 파괴되므로 s.rating은 undefined이다. (첨언하면, 기본 자료형은 메소드를 가지지 않는다고 했는데, 위 코드는 정상적으로 작동이 된다. 왜냐면 기본 자료형은 생성자 혹은 부모 객체를 가지고 있어서 기본 자료형의 객체를 만들기 위해 생성자를 사용한다. 자바스크립트는 문자열 객체에 있는 메소드에 접근하기 위해 기본 자료형을 문자열 객체로 형변환 시도, 그리고 필요한 작업이 끝나면 임시로 형변환된 객체는 가비지 컬렉터에 의해 제거된다)

## 3-3. 배열

* 배열 크기는 고정되지 않는다. 언제든 요소를 추가하거나 제거할 수 있다.
* 요소의 데이터 타입을 가리지 않는다.
* 배열 인덱스는 0으로 시작한다.

## 3-4. 날짜

자바스크립트의 날짜와 시간은 내장된 Date 객체에서 담당한다.

```javascript
const now = new Date();
now; //Tue Jul 16 2019 17:46:30 GMT+0900 (한국 표준시)
```

## 3-5. 정규표현식

자바스크립트의 정규표현식은 RegExp 객체를 사용한다.

```javascript
var re = /ab+c/;
```

## 3-6. 맵과 셋

맵은 객체와 마찬가지로 키와 값을 연결하지만, 특정 상황에서 객체보다 유리한 부분이 있다. 셋은 배열과 비슷하지만 중복이 허용되지 않는다. 위크맵과 위크셋은 맵과 셋 마찬가지로 동작하지만, 특정 상황에서 성능이 높아지도록 일부 기능을 제거한 버전이다.

# 4. 헷갈리는 개념들

## 4-1. null은 객체다

```js
console.log(typeof null) // object
```

결론적으로는 버그이다. typeof에서 null일 경우를 처리해줘야 하는데 처리를 안 해줘서 그냥 객체라고 출력하는 것이다.

이 같은 특성 때문에 null인지 아닌지 확인할 때는 다음과 같이 null과 직접 비교하는 것이 가장 좋다.

```js
var value = null;
console.log(value === null); // true
```

아니면 이렇게.

```js
var value = null;
console.log(!value); // true
```

## 4-2. NaN은 숫자다

NaN은 숫자형으로 정의되어있지만 숫자는 아니다. 그냥 스펙이 ECMAscript에 그렇게 정의되어있을 뿐...

# References

* [https://codeburst.io/javascript-essentials-types-data-structures-3ac039f9877b](https://codeburst.io/javascript-essentials-types-data-structures-3ac039f9877b)
* 러닝 자바스크립트
* 객체지향 자바스크립트의 원리
