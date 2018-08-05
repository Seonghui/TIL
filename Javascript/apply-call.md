call, apply
===
# 1.call과 apply를 사용하는 이유
함수가 실행될 때 내부 컨텍스트의 this는 실행 중인 객체 자신을 가리키거나 window를 가리킨다. 이때 this가 가리키는 대상을 바꿀 수 있는데, 이렇게 this값을 조작하는데 사용하는 방법이 바로 call()과 apply()이다.

```javascript
obj1 = {
    name: 'WEB',
    getName: function() {
        return this.name;
    }
};

obj2 = {
    name: 'FREE',
    getName: function() {
        return this.name;
    }
};

obj1.getName(); //"WEB"
obj1.getName.call(obj2); //"FREE"
```   

# 2. 사용방법
1. call
```
함수.call(지정할 객체명, 전달할 매개변수)
``` 

2. apply
```
함수.apply(지정할 객체명, [전달할 매개변수])
```

# 3. call()과 apply()의 차이점
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