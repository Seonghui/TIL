---
layout: default
title: React와 Vue
parent: etc
nav_order: 2
---

# React와 Vue

## 1. React.js의 특징

React의 모든 컴포넌트는 JSX를 사용한 렌더 함수를 통해 표현된다. JSX는 React를 위해 태어난 새로운 자바스크립트 문법으로, 과거 페이스북이 만들었던 PHP의 개량판 XHP에 그 기원을 두고 있다.

```js
class HelloMessage extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}
ReactDOM.render(<HelloMessage name="John" />, mountNode)
```

이 코드는 “Hello, John”라는 div를 생성하는 컴포넌트이다. React는 작성한 코드를 컴파일하는 과정을 꼭 거쳐야 한다. 대부분 사실상 자바스크립트 표준이 되어가고 있는 Babel을 사용한다. Babel의 React 플러그인을 통해 위의 코드를 컴파일하면 다음과 같은 모습이 된다.

```js
class HelloMessage extends React.Component {
  render() {
    return React.createElement(
      "div",
      null,
      "Hello ",
      this.props.name
    );
  }
}

ReactDOM.render(React.createElement(HelloMessage, { name: "John" }), mountNode);
```

이제 그냥 보통 자바스크립트 코드가 되었다. React는 Babel과 같은 트랜스파일러를 꼭 써야하기 때문에 Webpack 등을 통한 꽤 까다로운 초기 세팅이 필수적이다.

## 2. Vue.js의 특징

Vue.js는 여러모로 하이브리드형 프레임워크다. Angular의 양방향 바인딩과 React의 Virtual Dom 을 지원한다. 게다가 Flux 의 저장소 패턴을 구현한 vuex 를 통해 리액트의 단방향 데이터 바인딩도 활용 가능하다. jQuery 와 같이 cdn을 이용한 사용법까지 지원해서 유연성면에서는 가장 뛰어나며, 레거시 시스템을 점진적으로 개선할 때도 적합하다. vue.js의 .vue 파일(Single File Component)은 마크업과 스크립트, 스타일이 한 파일에 모여있다.

```text
<template>
  <div>
    <h1>{{ message }}</h1>
  </div>
</template>
<script>
  export default {
    data () {
      return {
        message: 'Hello World'
      }
    }
  }
</script>
<style>
  h1 {
    text-align: center;
  }
</style>
```

## 3. React와 Vue의 공통점

* 가상 DOM을 활용한다.
* 반응적이고 조합 가능한 컴포넌트를 제공한다.
* 코어 라이브러리에만 집중하고 있고 라우팅 및 전역 상태를 관리하는 컴패니언 라이브러리가 있다.
* 속도는 비슷하게 빠르다.

## 4. React와 Vue의 차이점

### 4-1. 최적화 방식

둘은 최적화 방식이 다르다. React에서는 컴포넌트의 state가 변경되면, 해당 컴포넌트를 루트로 시작해 전체 자식 컴포넌트들을 다시 렌더링(re-render) 한다. 자식 컴포넌트의 불필요한 렌더링을 줄이려면 `shouldComponentUpdate()` 메소드를 사용해야 한다. `shouldComponentUpdate()`는 자식 컴포넌트의 렌더링이 부모 컴포넌트로부터 받은 props에 의해 결정되게 해준다. 아래는 `shouldComponentUpdate()` 메소드를 사용해 자식 컴포넌트의 렌더링 여부를 결정하는 예제.

```js
class CounterButton extends React.Component {
  constructor(props) {
    super(props);
    this.state = {count: 1};
  }

  shouldComponentUpdate(nextProps, nextState) {
    // true를 반환할 때만 렌더링이 된다.
    if (this.props.color !== nextProps.color) {
      return true;
    }
    if (this.state.count !== nextState.count) {
      return true;
    }
    return false;
  }

  render() {
    return (
      <button
        color={this.props.color}
        onClick={() => this.setState(state => ({count: state.count + 1}))}>
        Count: {this.state.count}
      </button>
    );
  }
}
```

하지만 Vue는 렌더링을 할 때 컴포넌트의 종속성이 자동으로 추적된다. 따라서 상태가 변경될 때 실제로 어떤 컴포넌트를 다시 렌더링해야 하는지를 정확하게 알고 있다. 어떻게 보면 Vue의 컴포넌트들은 React의 `shouldComponentUpdate()`의 메소드를 가지고 있고, 이 메소드가 알아서 동작하는 것과 비슷한 느낌이라고 할 수 있다. 즉 최적화되지 않은 Vue의 업데이트는 최적화되지 않은 React보다 훨씬 빠르다.

### 4-2. JSX vs Template

#### JSX

React의 모든 컴포넌트는 JSX를 사용한 렌더 함수를 통해 표현된다. JSX를 사용하면 온전히 자바스크립트만을 이용해서 화면을 만들 수 있다. 따라서 같은 스코프 내에서 자바스크립트의 값을 직접적으로 가져다 쓸 수도(참조) 있다. Vue에서도 렌더링 함수와 JSX를 지원하지만 기본적으로는 템플릿을 사용한다. 아래는 JSX를 사용한 예시.

```html
<div id="app"></div>
```

```js
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      message: 'Hello React.js!'
    };
  }
  reverseMessage() {
    this.setState({
      message: this.state.message.split('').reverse().join('')
    });
  }
  render() {
    return (
      <div>
        <p>{this.state.message}</p>
        <button onClick={() => this.reverseMessage()}>
          Reverse Message
        </button>
      </div>
    )
  }
}
ReactDOM.render(App, document.getElementById('app'));
```

#### Template

템플릿은 HTML과 비슷하기 떄문에 러닝 커브가 낮고, HTML으로 작업한 기존 애플리케이션을 Vue로 이전하기가 쉽다. 아래는 템플릿 형식으로 작성한 예시.

```html
<template>
  <div id="app">
    <p>{{ message }}</p>
    <button v-on:click="reverseMessage">Reverse Message</button>
  </div>
</template>
```

```js
new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('');
    }
  }
});
```

### 4-3. React와 Vue의 불변성

React에서 state 는 불변(immutable)의 속성을 가진다. 그래서 직접적으로 변경할 수 없다. 만약 상태를 변경하고 싶다면 아래처럼 `setState()`를 사용해야 한다.

```js
this.setState({
    message: this.state.message.split('').reverse().join('')
});
```

React는 현재와 이전 상태를 비교하여 언제, 어떻게 다시 DOM 을 렌더링 할지 결정한다. 그렇기 때문에 불변하는 속성이 필요하다.

반대로, Vue 에서 data 는 변경될 수 있다. 아래는 위의 React 의 message 와 같은 data 속성을 직접 변경하는 모습이다.

```js
// 아래의 message 속성은 Vue 인스턴스에서 접근 할 수 있습니다.
this.message = this.message.split('').reverse().join('');
```

Vue의 렌더링 시스템이 React보다 효율성이 떨어진다고 결론짓기 전에, Vue가 실제로 어떻게 렌더링을 하는지 더 자세히 살펴보자. state 에 새로운 객체를 추가했을 때, Vue 가 해당 객체의 모든 속성을 확인하고 나서 getter 와 setter 로 변환한다. 그리고 Vue의 reactivity system 이 모든 상태를 모니터링하여 변경이 일어날 때마다 자동으로 DOM 을 다시 렌더링한다.

## References

* [Vue.js - Comparison with Other Frameworks](https://vuejs.org/v2/guide/comparison.html)
* [React.js - 성능 최적화](https://ko.reactjs.org/docs/optimizing-performance.html#shouldcomponentupdate-in-action)
* [아하 프론트 개발기(0)— Angular, React, Vue.js 프레임워크 선택하기](https://medium.com/aha-official/%EC%95%84%ED%95%98-%ED%94%84%EB%A1%A0%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EA%B8%B0-0-angular-react-vue-js-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC-%EC%84%A0%ED%83%9D-f797392118d0)
* [React 인가 Vue 인가?](https://joshua1988.github.io/web_dev/vue-or-react/)
* [React의 탄생배경과 특징](https://medium.com/@RianCommunity/react%EC%9D%98-%ED%83%84%EC%83%9D%EB%B0%B0%EA%B2%BD%EA%B3%BC-%ED%8A%B9%EC%A7%95-4190d47a28f)
