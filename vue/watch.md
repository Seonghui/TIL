watch로 데이터 변화 감지하기
---
watch 속성의 handler 함수는 배열이 변화하면 발동되는 콜백함수임.
object 내부의 데이터값을 감지하려면 deep이라는 프로퍼티를 사용해야 됨. 참고로 화살표 함수 사용할 수 없음. 부모 컨텍스트를 바인딩하지 않기 떄문.


```javascript
var vm = new Vue({
  data: {
    temp: 1,
  },
  watch: {
    temp: {
      handler: function (val, oldVal) { /* ... */ },
      deep: true
    }
})
```

* https://www.amuponda.com/2018/06/24/vue-js-watch-changes-to-an-array-of-objects/
* https://stackoverflow.com/questions/35755027/how-to-call-function-from-watch