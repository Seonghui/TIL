---
description: 어떻게 자바스크립트 코드가 실행되는지 이해하기 위해 알아야하는 개념이다.
---

# 실행 컨텍스트 \(실행 문맥\)

## 1. 실행 컨텍스트 개념

### 1-1. 메모리 힙과 콜 스택

![&#xBA54;&#xBAA8;&#xB9AC; &#xD799;&#xACFC; &#xD638;&#xCD9C; &#xC2A4;&#xD0DD;](https://developer.mozilla.org/files/4617/default.svg)

* 메모리 힙: 객체는 힙\(구조화되지 않은 메모리 영역\)에 할당됨. 변수와 객체에 대한 모든 메모리 할당은 여기서 발생
* 콜 스택: 코드가 실행될 때 콜 스택이 쌓임.\(실행 컨택스트\)

### 1-2. 콜 스택 및 실행 컨텍스트

함수를 호출할 때 해당 함수의 호출 정보\(함수 내 지역 변수 혹은 인자값 등\)가 차곡차곡 쌓여있는 스택을 콜 스택\(호출 스택\)이라고 한다. C 언어에서는 이러한 콜 스택의 호출 정보 등으로 코드의 실행 과정을 추적하여 디버깅과 같은 작업을 수행한다.  
실행 컨텍스트는 콜 스택에 들어가는 실행 정보와 비슷하다. 실행 컨텍스트는 실행 가능한 코드를 형상화하고 구분하는 추상적인 개념으로, 더 쉽게 말하자면 코드 덩어리\(코드 블록\)같은 것이다. 여기서 말하는 코드 덩어리는 대부분의 경우 함수가 된다.

대부분은 함수로 실행 컨텍스트를 만든다. 그리고 이 코드 블록 안에 변수 및 객체, 실행 가능한 코드가 들어있다. 이 코드가 실행되면 실행 컨텍스트가 생성되고, 실행 컨텍스트는 스택 안에 하나씩 차곡차곡 쌓이고, 제일 위에 위치하는 실행 컨텍스트가 현재 실행되고 있는 컨텍스트가 된다.

현재 실행되고 있는 컨텍스트에서 이 컨텍스트와 관련 없는 실행 코드가 실행되면, 새로운 컨텍스트가 생성되어 스택에 들어가고 그 제어권이 컨텍스트로 이동한다.

## 2. 실행 컨텍스트가 형성되는 경우

ECMAScript에서는 실행 컨텍스트가 형성되는 경우를 세 가지로 규정하고 있다.

1. 전역 코드
2. eval\(\) 함수로 실행되는 코드
3. 함수 안의 코드를 실행한 경우

### 2-1. 전역 코드 및 함수 안의 코드를 실행한 경우의 예시

```javascript
console.log("This is global context");

function ExContext1() {
  console.log("This is ExContext1");
}

function ExContext2() {
  ExContext1();
  console.log("This is ExContext2");
}

ExContext2()
```

위 코드를 실행하면 아래와 같이 실행 컨텍스트 스택\(Stack\)이 생성하고 소멸한다. 현재 실행 중인 컨텍스트에서 이 컨텍스트와 관련없는 코드\(예를 들어 다른 함수\)가 실행되면 새로운 컨텍스트가 생성된다. 이 컨텍스트는 스택에 쌓이게 되고 컨트롤\(제어권\)이 이동한다.

![&#xC2E4;&#xD589; &#xCEE8;&#xD14D;&#xC2A4;&#xD2B8;](https://ppunmg.bn.files.1drv.com/y4mGdpThhx8MyTEu7wvEnl8WTECHKhBs3S1nOvaoK7JAdJy-PoOq4VWN7qHHaSpp4OkZa_DRv_xUuJZGp9VVcE52ds60Yu-WptYoXcl0Uu_xmn2EqW7c7FDESa43kpskysCHCN9j3v0Q0joG-mzxDZy9er4GuwUsU5UvmBmm8JwM4D0Cx7LZ15lS6zHu50l1BT86Vmx57h1iDg5UgBMEMdq-w?width=1404&height=362&cropmode=none)

## 3. 실행 컨텍스트의 3가지 객체

실행 컨텍스트는 실행 가능한 코드를 형상화하고 구분하는 추상적인 개념이지만 물리적으로는 객체의 형태를 가지며 아래의 3가지 프로퍼티를 소유한다.

### 3.1 Variable Object\(변수 객체, 활성 객체\)

* 변수
* 매개변수\(parameter\)와 인자\(argument\)
* 함수 선언 \(함수 표현식은 제외\)

변수 객체에는 위와 같은 정보를 담을 수 있다. 실행 컨텍스트가 생성되면 자바스크립트 엔진은 해당 컨텍스트에서 실행에 필요한 여러 가지 정보를 담을 객체를 생성하는데, 이를 변수 객체라고 한다. 변수 객체는 코드가 실행될 때 엔진에 의해 참조되며 코드에서는 접근할 수 없다. 이 객체에 앞으로 사용하게 될 매개변수나 사용자가 정의한 변수 및 객체를 저장하고, 새로 만들어진 컨텍스트로 접근 가능하게 되어있다.

변수 객체는 실행 컨텍스트의 프로퍼티이기 때문에 값을 갖는데 이 값은 다른 객체를 가리킨다. 그런데 전역 코드 실행시 생성되는 전역 컨텍스트의 경우와 함수를 실행할 때 생성되는 함수 컨텍스트의 경우, 가리키는 객체가 다르다. 이는 전역 코드와 함수의 내용이 다르기 때문이다. 예를 들어 전역 코드에는 매개변수가 없지만 함수에는 매개변수가 있다.

### 3.2 Scope Chain \(스코프 체인\)

일종의 리스트이다. 전역 객체와 중첩된 함수의 스코프의 레퍼런스를 차례로 저장하고 있다. 다시 말해, 스코프 체인은 해당 전역 또는 함수가 참조할 수 있는 변수, 함수 선언 등의 정보를 담고 있는 전역 객체\(GO\) 또는 활성 객체\(AO\)의 리스트를 가리킨다. 현재 실행 컨텍스트의 변수 객체를 선두로 하여 상위 컨텍스트의 변수 객체를 가리키며 마지막 리스트는 전역 객체를 가리킨다.

엔진은 스코프 체인을 통해서 변수의 스코프를 파악한다. 함수가 중첩 상태일 때 하위함수 내에서 상위함수의 유효 범위까지 참조할 수 있는데 이는 스코프 체인을 검색했기 때문이다. 함수가 중첩되어 있으면 중첩될 때마다 부모 함수의 Scope가 자식 함수의 스코프 체인에 포함된다. 함수 실행중에 변수를 만나면 그 변수를 우선 현재 Scope, 즉 변수 객체에서 검색해보고, 만약 검색에 실패하면 스코프 체인에 담겨진 순서대로 그 검색을 이어가게 되는 것이다. 이것이 스코프 체인이라고 불리는 이유이다.

예를 들어 함수 내의 코드에서 변수를 참조하면 엔진은 스코프 체인의 첫번째 리스트가 가리키는 변수 객체에 접근하여 변수를 검색한다. 만일 검색에 실패하면 다음 리스트가 가리키는 변수 객체\(또는 전역 객체\)를 검색한다. 이와 같이 순차적으로 스코프 체인에서 변수를 검색하는데 결국 검색에 실패하면 정의되지 않은 변수에 접근하는 것으로 판단하여 Reference 에러를 발생시킨다. 스코프 체인은 함수의 감춰진 프로퍼티인 \[\[Scope\]\]로 참조할 수 있다.

### 3.3 this

`this` 프로퍼티에는 this 값이 할당된다. `this`에 할당되는 값은 this 바인딩의 호출 패턴에 의해 결정된다.

## 4. 실행 컨텍스트 생성 과정

실행 컨텍스트에 진입하기 이전 유일한 전역 객체\(Global Object\)가 생성된다. 전역 객체는 단일 사본으로 존재하며 이 객체의 프로퍼티는 코드의 어떠한 곳에서도 접근할 수 있다. 초기 상태의 전역 객체에는 빌트인 객체\(Math, String, Array 등\)와 BOM, DOM이 설정되어 있다. 전역 객체가 생성된 이후에는 전역 실행 컨텍스트가 생성되고, 실행 컨텍스트 스택에 쌓인다.

이후 함수가 호출되면 함수의 실행 컨텍스트가 생성된다. 실질적으로는 함수 내부 정보를 담는 객체가 생성되고, 그 안에는 함수 정보가 담긴다. 이후 코드가 생성된다.

![execution-context](https://user-images.githubusercontent.com/16531837/44141222-71a427bc-a0b7-11e8-8b90-8257d70b2ffc.png)

### 4.1 변수 객체 생성

실행 컨텍스트가 생성되면 자바스크립트 엔진은 해당 컨텍스트에서 실행에 필요한 여러 가지 정보를 담을 객체를 생성하는데, 이를 변수 객체라고 한다. 이 객체에 앞으로 사용하게 될 매개변수나 사용자가 정의한 변수 및 객체를 저장하고, 새로 만들어진 컨텍스트로 접근 가능하게 되어 있다. 이는 엔진 내부에서 접근할 수 있다는 것이지 사용자가 접근할 수 있다는 것은 아니다.

### 4.2 arguments 객체 생성

`arguments` 프로퍼티는 `arguments` 객체를 참조한다.

### 4.3 스코프 정보 생성

현재 컨텍스트의 유효 범위를 나타내는 스코프 정보를 생성한다. 이 스코프 정보는 현재 실행 중인 실행 컨텍스트 안에서 연결 리스트와 유사한 형식으로 만들어진다. 현재 컨텍스트에서 특정 변수에 접근해야 할 경우 이 리스트를 활용한다. 이 리스트에서 찾지 못한 변수는 결국 정의되지 않은 변수에 접근하는 것으로 판단하여 에러를 검출한다. 이 리스트를 스코프 체인이라고 한다. \(현재 생성된 변수 객체가 스코프 체인의 제일 앞에 추가된다.\)

### 4.4 변수 생성

현재 실행 컨텍스트 내부에서 사용되는 지역 변수의 생성이 이루어진다. 앞서 생성된 변수 객체가 변수 객체로 사용된다. 변수 객체 안에서 호출된 함수 인자는 각각의 프로퍼티가 만들어지고 그 값이 할당된다. 하지만 변수나 내부 함수를 단지 메모리에 생성하고, 초기화는 변수나 함수에 해당하는 표현식이 실행되기 전까지는 이루어지지 않는다. 따라서 변수값에 `undefined`가 할당된다.

### 4.5 this 바인딩

`this` 키워드를 사용하는 값이 할당된다. 여기서 `this`가 참조하는 객체가 없으면 전역 객체를 참조한다.

### 4.6 코드 실행

이렇게 하나의 실행 컨텍스트가 생성되고, 변수 객체가 만들어진 후에 코드에 있는 여러 가지 표현식 실행이 이루어진다. 이렇게 실행되면서 변수의 초기화 및 연산, 또 다른 함수 실행등이 이루어진다. 그리고 3.4에서 생성했던 변수에도 값이 할당된다.

## 5. 전역 실행 컨텍스트와 일반적인 실행 컨텍스트

참고로 전역 실행 컨텍스트는 일반적인 실행 컨텍스트와는 약간 다르다. arguments 객체가 없으며 전역 객체 하나만을 포함하는 스코프 체인이 있다. 또한 변수 객체가 가리키는 것은 전역 객체가 된다.

## refs

* 인사이드 자바스크립트
* [poiemaweb](https://poiemaweb.com/js-execution-context)
