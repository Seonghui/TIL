# hoisting

## 호이스팅

## 호이스팅의 정의

* hoist 의 사전적 정의는 '끌어올리다'.
* 후선언된 변수나 함수가 해당 Scope 에서 최상위로 끌어올려지는 것을 뜻한다. 즉 변수 선언문이 유효범위 안의 제일 상단부로 끌어올려지게 되고, 선언문이 있던 자리에서 초기화가 이루어지는 결과를 갖는다. 그 덕에 뒤에서 선언된 함수들과 변수들을 그 전에 사용할 수 있다.
* 여기서 끌어올려지는 것은 할당이 아니라 **선언**이다.
* **함수 선언문** 방식일 때만 호이스팅이 된다. 함수 표현식으로 정의한 함수는 호이스팅이 되지 않는다.

### 예시 1

```javascript
add(2, 3); //5

function add(x, y) {
  return x + y;
}

add(3, 4); //7
```

add\(\) 함수가 정의되지 않았음에도 불구하고 add\(2,3\)은 제대로 호출된다. add\(\) 함수가 최상위로 끌어올려졌기 때문이다.  
호이스팅에 의해 자바스크립트 인터프리터는 코드를 아래와 같이 인식한다.

```javascript
function add(x, y) {
  return x + y;
}

add(2, 3);
add(3, 4);
```

### 예시 2

```javascript
function myFunc() {
  console.log(x); //undefined
  var x = 10;
  console.log(x); //10
}

myFunc();
```

다른 언어의 경우에는 변수 x 를 선언하지 않고 출력하면 오류 메세지를 출력한다. 하지만 자바스크립트에서는 undefined 라고 출력되고 그냥 넘어간다. var x;가 스코프의 최상단으로 끌어올려졌기 때문이다\(호이스팅\). 작동 순서에 맞게 코드를 재구성하면 다음과 같다.

```javascript
function myFunc() {
  var x;
  console.log(x); //undefined
  x = 10;
  console.log(x); //10
}

myFunc();
```

