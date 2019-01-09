vue.js 데이터
---
# What is difference between 'data:' and 'data()' in Vue.js?
전체적으로는 같음

그냥 일반적인 상황에서는 아래와 같이 사용해도 문제는 없음.
```js
data: {
  count: 0
}
```

그런데 [컴포넌트에서는 데이터는 무조건 함수여야..](https://vuejs.org/v2/guide/components.html#data-Must-Be-a-Function) 그래야 각 인스턴스에서 리턴된 데이터 오브젝트의 독립적인 복사본을 관리할 수 잇다고 함. 이게 무슨 말이냐면 인스턴스는 재사용이 가능하고, 컴포넌트가 재사용이 될때마다 데이터는 각각 unique object가 되어야 함. 그런데 위와 같이 object로 선언하면 컴포넌트를 재사용할때마다 같은 값이 공유되기 때문에 뷰 공식 문서에 있는 예시처럼 클릭할 때마다 다른 재사용된 컴포넌트들한테도 영향을 미치게 되는 것임. 

그리고 data function에서 절대 화살표 함수 사용하지 말자.. 화살표 함수 사용하면 this가 현재 스코프로 바뀌게 되니깐요..(아마 window가 될 것임)
```js
data: function () {
  return {
    count: 0
  }
}
```

아무튼 data()로 선언해야 초기화가 가능... 원본 객체를 대상으로 해야되는데 그냥 data:로 선언을 해버리면 원본 객체가 바뀌게 되어서 그런 듯
```js
Object.assign(this.$data, this.$options.data.apply(this));
```