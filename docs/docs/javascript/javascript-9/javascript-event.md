---
layout: default
title: 자바스크립트 이벤트
parent: 9. 기타
grand_parent: javascript
nav_order: 30
---


# 자바스크립트 이벤트 리스너

{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
{:toc}

---

자바스크립트의 이벤트(event)란 웹 브라우저가 알려주는 HTML 요소에 대한 사건의 발생을 의미한다. 웹 페이지에 사용된 자바스크립트는 이렇게 발생한 이벤트에 반응하여 특정 동작을 수행할 수 있다.

## 이벤트 리스너

이벤트 리스너란 이벤트가 발생했을 때 그 처리를 담당하는 함수를 가리키며, 이벤트 핸들러(event handler)라고도 합니다.
이벤트 프로그래밍을 하기 위해서는 이벤트의 대상에 이벤트 리스너를 등록해줘야 한다. 대상에 이벤트를 설치한다고 보면 된다. 웹브라우저에서는 크게 3가지 종류의 등록방법을 제공한다.

* Inline
* Property Listener
* AddEvnetListener

### Inline

태그 안에 이벤트가 속성으로 직접 들어가있을 때를 인라인 이벤트라고 한다.

```html
<input type="button" onclick="alert('Hello world');" value="button" />
```

인라인 코드 내의 this는 이 이벤트가 동작하고 있는 element(자기 자신)를 가리키게 된다.

```html
<input type="button" onclick="alert('Hello world, '+this.value);" value="button" />
```

인라인 방식은 태그에 이벤트가 포함되기 때문에 이벤트의 소재를 파악하는 것이 편리하다. 하지만 정보인 HTML과 제어인 JavaScript가 혼재된 형태이기 때문에 바람직한 방법이라고 할수는 없다.

### Property Listener

프로퍼티 리스너 방식은 이벤트 대상에 해당하는 객체의 프로퍼티로 이벤트를 등록하는 방식이다.

이 방법의 단점은 이벤트 타입별로 오직 하나의 이벤트 리스너만을 등록할 수 있다는 점이다.

```html
<input type="button" id="target" value="button" />
<script>
    var t = document.getElementById('target');
    t.onclick = function(){
      alert('Hello world');
    }
</script>
```

이벤트가 실행된 맥락의 정보가 필요할 때는 이벤트 객체를 사용한다. 이벤트 객체는 이벤트가 실행될 때 이벤트 핸들러의 인자로 전달된다.

```html
<body>
    <input type="button" id="target" value="button" />
<script>
    var t = document.getElementById('target');
    t.onclick = function(event){
        alert('Hello world, '+event.target.value)
    }
</script>
```

### AddEvnetListener

addEventListener은 이벤트를 등록하는 가장 권장되는 방식이다. 이 방식을 이용하면 여러개의 이벤트 핸들러를 등록할 수 있다.

#### 구문

```text
target.addEventListener(type, listener[, options]);
target.addEventListener(type, listener[, useCapture]);
```

##### 매개변수

* type: 반응할 이벤트 유형을 나타내는 대소문자 구분 문자열.
* listener 지정된 이벤트가 발생했을 때 실행할 이벤트 리스너를 전달한다. 주로 콜백 함수가 된다.
* options: 이벤트 리스너에 대한 특성을 지정하는 옵션 객체이다. 사용 가능한 옵션은 [여기](https://developer.mozilla.org/ko/docs/Web/API/EventTarget/addEventListener) 참고.
* useCapture: 이벤트 전파 방식에 대한 옵션이다. false면 버블링(bubbling) 방식으로, true면 캡처링(capturing) 방식으로 이벤트를 전파한다.

#### AddEvnetListener()로 이벤트 리스너 등록하기

```html
<input type="button" id="target" value="button" />
<script>
    var t = document.getElementById('target');
    t.addEventListener('click', function(event){
        alert('Hello world, '+event.target.value);
    });
</script>
```

#### AddEvnetListener()로 이벤트 리스너 여러 개 등록하기

```html
<button id="btn">버튼</button>
<p id="text"></p>
<script>
// 아이디가 "btn"인 요소를 선택함.
var btn = document.getElementById("btn");

// 선택한 요소에 click 이벤트 리스너를 등록함.
btn.addEventListener("click", clickBtn);

// 선택한 요소에 mouseover 이벤트 리스너를 등록함.
btn.addEventListener("mouseover", mouseoverBtn);

// 선택한 요소에 mouseout 이벤트 리스너를 등록함.
btn.addEventListener("mouseout", mouseoutBtn);

function clickBtn() {
  document.getElementById("text").innerHTML = "버튼이 클릭됐어요!";
}

function mouseoverBtn() {
  document.getElementById("text").innerHTML = "버튼 위에 마우스가 있네요!";
}

function mouseoutBtn() {
  document.getElementById("text").innerHTML = "버튼 밖으로 마우스가 나갔어요!";
}
</script>
```

## 이벤트 호출

이벤트 리스너가 등록되고 해당 객체나 요소에 지정된 타입의 이벤트가 발생하면, 브라우저는 자동으로 등록된 이벤트 리스너를 호출한다.

이때 이벤트 리스너는 인수로 이벤트 객체(event object)를 전달받으며, 식별자를 통해 전달받은 이벤트 객체를 참조한다.

### 이벤트 객체

이벤트 객체(event object)란 특정 타입의 이벤트와 관련이 있는 객체이다. 이벤트 객체는 해당 타입의 이벤트에 대한 상세 정보를 저장하고 있다.

모든 이벤트 객체는 이벤트의 타입을 나타내는 type 프로퍼티와 이벤트의 대상을 나타내는 target 프로퍼티를 가지고, 이러한 이벤트 객체는 이벤트 리스너가 호출될 때 인수로 전달된다.

```html
<h1>이벤트 객체</h1>
<button id="btn">눌러보세요!</button>
<p id="text"></p>

<script>
  // 아이디가 "btn"인 요소를 선택함.
  var btn = document.getElementById("btn");
  // 선택한 요소에 click 이벤트 리스너를 등록함.
  btn.addEventListener("click", clickBtn);
  
  function clickBtn(event) {
    document.getElementById("text").innerHTML =
      "이 이벤트의 타입은 " + event.type + "이며, 이벤트의 대상은 " + event.target + "입니다.";
  }
</script>
```

### 이벤트 전파

이벤트 전파방식은 크게 두 가지로 나뉜다.

* 버블링 전파 방식: 하위에서 상위로
* 캡처링 전파 방식: 상위에서 하위로

이벤트 전파방식에 대한 자세한 내용은 [여기](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/) 참고.

### 이벤트 취소

기본 동작 취소

```js
event.preventDefault();
```

전파 취소

```js
event.stopPropagation();
```

## References

* [생활코딩 - 이벤트](https://opentutorials.org/course/1375/6629)
* [MDN - EventTarget.addEventListener()](https://developer.mozilla.org/ko/docs/Web/API/EventTarget/addEventListener)
* [tcpschool - 이벤트 리스너 등록](http://tcpschool.com/javascript/js_event_eventListenerRegister)
