# vue.js 스타일 가이드

## 필수

### 컴포넌트 이름은 합성어로 \(App 제외\)

웬만하면 약어는 쓰지 말자

```text
todo (x)
TodoItem(o)
```

### 컴포넌트 데이터는 무조건 함수로 함

data 값이 오브젝트일 경우 컴포넌트의 모든 인스턴스가 공유. 그런데 각 컴포넌트가 자체 데이터를 관리하게끔 하려면 고유한 data 객체를 생성해야 하는데 함수안에서 객체를 반환하는 방법으로 해결하면 됨

```text
// 오브젝트
data: {
  listTitle: '',
  todos: []
}

// 함수
data: function () {
  return {
    listTitle: '',
    todos: []
  }
}
```

### v-for에 키값 지정해야 함

키는 각각의 노드에 고유한 아이디를 지정해줌. key 값을 사용하면 특정 엘리먼트만 업데이트할 수 있음. \(최적화 가능, [링크 참고하기](https://codepen.io/beomy/pen/WYMGJB?editors=1010)\)

```text
// bad
<ul>
  <li v-for="todo in todos">
    {{ todo.text }}
  </li>
</ul>

// good
<ul>
  <li
    v-for="todo in todos"
    :key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```

### v-for랑 v-if 동시에 사용하지 말기

만약 꼭 써야되는경우 목록 자체를 필터링해서 할것

```text
// bad
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id"
  >
    {{ user.name }}
  <li>
</ul>

// good
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id"
  >
    {{ user.name }}
  <li>
</ul>
```

## 강추

### 싱글 파일 컴포넌트 파일 이름

파일네임은 케밥케이스 아니면 파스칼케이스로 작성할 것. 그런데 파스칼케이스가 더 나음. 그리고 템플릿에다가도 파스칼케이스를 쓰자. 헷갈리니까... \(단, 돔 템플릿은 약간 다른데 이건 돔 템플릿 사용할때 찾아보기\)

```text
// bad
components/
|- mycomponent.vue
components/
|- myComponent.vue

// Good
components/
|- MyComponent.vue
components/
|- my-component.vue
```

### 웬만하면 접두사 붙일 것

부모가 있는 자식컴포넌트의 경우 부모이름을 접두사로 붙인다. 이름은 상위레벨부터 작성하고 마지막은 설명적인 단어로 끝낸다.

```text
// bad
components/
|- MyButton.vue
|- VueTable.vue
|- Icon.vue

// good
components/
|- BaseButton.vue
|- BaseTable.vue
|- BaseIcon.vue
components/
|- AppButton.vue
|- AppTable.vue
|- AppIcon.vue
components/
|- VButton.vue
|- VTable.vue
|- VIcon.vue
```

### 내용을 전달받지 못한 컴포넌트는 셀프 클로징

```text
<MyComponent/>
```

### 여러개의 속성이 있으면 여러줄로 쓰자

줄 하나에 하나씩

```text
// Bad
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">
<MyComponent foo="a" bar="b" baz="c"/>

// Good
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>
<MyComponent
  foo="a"
  bar="b"
  baz="c"
/>
```

### 뭐든 계산해야하는건 템플릿에다 적지말자

템플릿에다 하면 복잡해짐. 심플한 코드와 재사용성을 위해 computed나 filter로...


### 복잡한 계산은 최대한 간단한 계산으로 분할할 것

쉬운 테스트, 가독성, adaptable 때문이라도 분할하기

```text
// Bad
computed: {
  price: function () {
    var basePrice = this.manufactureCost / (1 - this.profitMargin)
    return (
      basePrice -
      basePrice * (this.discountPercent || 0)
    )
  }
}

//Good
computed: {
  basePrice: function () {
    return this.manufactureCost / (1 - this.profitMargin)
  },
  discount: function () {
    return this.basePrice * (this.discountPercent || 0)
  },
  finalPrice: function () {
    return this.basePrice - this.discount
  }
}
```

### 비어있지않은 HTML 속성은값은 "" 안에 집어넣을것

```text
// Bad
<input type=text>
<AppSidebar :style={width:sidebarWidth+'px'}>

// Good
<input type="text">
<AppSidebar :style="{ width: sidebarWidth + 'px' }">
```

### 코드 일관성 유지하기

Directive shorthands \(: for v-bind: and @ for v-on:\) 쓸거면 쓰고 말거면 말기. 섞어서 쓰지 말자.

## 그밖에 알면 있으면 좋을것들

### 컴포넌트 옵션 순서

el, components, filters, extends, mixins, props, propsData, data, computed, watch, lifecycle, method 순서로... 더 많지만 자세한 내용은 여기 참고 [클릭](https://vuejs.org/v2/style-guide/#Full-word-component-names-strongly-recommended)

### element 속성 순서

is, v-for, v-if, v-show, v-choak, v-pre, v-once, id, ref, key, slot, v-model, v-on, v-html, v-text

