# 변수 \(Variable\)

* 자바스크립트 변수는 값을 복사하거나 참조하는 방식으로 데이터를 담음
* 기본이 되는 값은 무엇이든 변수로 복제
* 할당되고 복사되고 함수에 전달하거나 반환할 때 값으로 처리됨
* 이외에는 참조하는 방식인데, 값을 담지 않는다면 객체를 참조함
* 여기서 참조값은 객체가 메모리에 담긴 주소값을 가리키는 포인터
* 단, 문자열을 변경시에는 문자열 수정이 아닌 새로운 객체가 됨 \(3번 예시 참고\)

## 예시

### 1. 단일 객체를 여러 변수가 참조하는 예시

* 객체의 실제 값을 복사하지 않은 채, 단순히 똑같은 객체를 가리키는 경우임
* 따라서 두 개의 변수가 똑같은 객체를 가리키고 있음
* 한 개의 참조 값을 통해 참조하는 값을 갱신하면 다른 참조 값에도 반영

```javascript
// 객체 생성
var obj = {};

// 객체 참조
var refToObj = obj;

// 객체 속성 변경
obj.onProperty = true;

console.log(refToObj.onProperty === obj.onProperty); // true

// obj 객체를 참조한 변수에 프로퍼티 추가 (갱신)
refToObj.anotherProperty = 1;

console.log(refToObj.anotherProperty === obj.anotherProperty); // true
```

### 2. 객체의 참조값을 수정했지만 본래의 상태를 유지하는 경우

* 참조 값은 다른 참조 값이 아니라 오로지 참조되는 객체만을 가리킨다
* 이게 뭔 말이냐면 객체 실체가 바뀌었지만 참조한 값은 여전히 이전의 객체를 가리키는 경우임

  \`\`\`js

  // 배열 생성

  var items = \['one', 'two', 'three'\];

  // 참조

  var itemRef = items;

// items 를 다른 객체로 변경 items = \['new', 'array'\];

console.log\(items !== itemRef\); // true

console.log\(items\); // \['new', 'array'\] console.log\(itemRef\); // \['one', 'two', 'three'\]

```text
## 3. 문자열 객체를 수정하자 새로운 객체가 만들어진 예시
```js
// 문자열 객체 생성
var item = 'test';

// 참조
var itemRef = item;


// 수정
// 이렇게 수정하면 원시 객체를 수정하지 않고 새로운 객체가 만들어짐
item += 'ing';

console.log(item != itemRef); // true
```

## ref

* 모던 자바스크립트 테크닉

