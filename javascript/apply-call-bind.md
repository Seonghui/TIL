# apply-call-bind

## call, apply, bind, Spread Operator

## 1. call과 apply

함수가 실행될 때 내부 컨텍스트의 this는 실행 중인 객체 자신을 가리키거나 window를 가리킨다. 이때 this가 가리키는 대상을 바꿀 수 있는데, 이렇게 this값을 조작하는데 사용하는 방법이 바로 call\(\)과 apply\(\)이다.

```javascript
var obj1 = {
  name: 'WEB',
  getName: function() {
    return this.name;
  }
};

var obj2 = {
  name: 'FREE',
  getName: function() {
    return this.name;
  }
};

obj1.getName(); //'WEB'
obj1.getName.call(obj2); //'FREE'
```

### 1.1 사용방법

* call

```text
함수.call(지정할 객체명(컨텍스트), 전달할 매개변수)
```

* apply

```text
함수.apply(지정할 객체명(컨텍스트), [전달할 매개변수])
```

### 1.2 call\(\)과 apply\(\)의 차이점

동일하게 this 키워드가 가리키는 대상을 변경할 수 있게 해주지만, 인자를 전달하는 방식에 차이가 있다.

* call: 쉼표로 구분해서 전달
* apply: 배열 형태로 전달

```javascript
var myobj = {
  sum: function(a, b) {
    return a + b;
  }
};

myobj.sum.call(null, 1, 2); // 3
myobj.sum.apply(null, [1, 2]); // 3
```

### 1.3 확산 연산자\(Spread Operator\)

만약 this 값이 중요하지 않은 경우 확산 연산자를 그대로 사용할 수 있다. 이 경우 apply와 같은 결과를 얻을 수 있다. 아래 예제에서 this의 값에 null을 쓴 이유는 Math.min과 Math.max가 this와 관계없이 동작하기 때문이다. 즉, 무엇을 넘기든 상관이 없다.

```javascript
const arr = [2, 3, -5, 15, 7];
Math.min(arr); // NaN
Math.min.apply(null, arr); // -5
Math.min(...arr); // -5
Math.max.apply(null, arr); // 15
Math.max(...arr); //15
```

## 2. bind

함수의 this 값을 영구적으로 바꿀 수 있다.

```javascript
var obj1 = {
  name: 'WEB',
  getName: function() {
    return this.name;
  }
};

var obj2 = {
  name: 'FREE',
  getName: function() {
    return this.name;
  }
};

var res = obj1.getName.bind(obj2);
res(); // 'FREE'
```

## Example

```javascript
const bruce = {
    name: 'Bruce'
  };

  const madeline = {
    name: 'Madeline'
  };

  function greet() {
    console.log(`Hello, I'm ${this.name}`);
  };

  greet(); // undefined
  greet.call(bruce); // Hello, I'm Bruce
  greet.call(madeline); // Hello, I'm Madeline

  function update(birthYear, occupation){
    this.birthYear = birthYear;
    this.occupation = occupation;
  };

  /* call 과 apply 호출
  ***/ 
  update.call(bruce, 1949, 'singer');
  update.call(madeline, 1942, 'actress');

  console.log(bruce); 
  // {name: "Bruce", birthYear: 1949, occupation: "singer"}
  console.log(madeline); 
  // {name: "Madeline", birthYear: 1942, occupation: "actress"}

  update.apply(bruce, [1955, 'actor']);
  update.apply(madeline, [1918, 'writer']);

  console.log(bruce); 
  // {name: "Bruce", birthYear: 1955, occupation: "actor"}
  console.log(madeline); 
  // {name: "Madeline", birthYear: 1918, occupation: "writer"}

  /* Spread operator 를 사용해도 apply와 같은 결과를 얻을 수 있다.
  ***/ 
  newBruce = [1940, "martial artist"];
  update.call(bruce, ...newBruce); 
  // apply(bruce, newBruce)와 같음
  console.log(bruce); 
  // {name: "Bruce", birthYear: 1940, occupation: "martial artist"}

  /* bind를 사용해 this.name의 값을 항상 bruce로 바꿀 수 있다.
  ***/ 
  const updateBruce = update.bind(bruce);
  updateBruce(1904, "actor");
  console.log(bruce); 
  //{ name: "Bruce", birthYear: 1904, occupation: "actor" }

  /* call을 호출해 this.name을 변경해도 Madeline으로 변하지 않는다.
  ***/ 
  updateBruce.call(madeline, 1274, "king");
  console.log(bruce); 
  // { name: "Bruce", birthYear: 1274, occupation: "king" };

  /* bruce가 태어난 해를 1949로 고정하고, 직업은 자유롭게 바꾸는 업데이트 함수
  ***/ 
  const updateBruce1949 = update.bind(bruce, 1949); updateBruce1949("singer, songwriter");
  console.log(bruce); 
  // {name: "Bruce", birthYear: 1949, occupation: "singer, songwriter"}
```

