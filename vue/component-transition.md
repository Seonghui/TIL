컴포넌트 트랜지션
---

```html
<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>
```

```js
new Vue({
  el: '#demo',
  data: {
    show: true
  }
})
```

```css
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
```

* transition 주고 싶은 element를 transition 컴포넌트로 감싸기.
* 다음에 자바스크립트에서 훅을 제공하면 적절한 타이밍에 호출. 
* 트랜지션 고유의 클래스가 있음
* keyframes 사용 가능
* 최초 렌더링시 트랜지션 적용하려면 appear 속성 추가

# References
* https://kr.vuejs.org/v2/guide/transitions.html