---
layout: default
title: React와 Vue
parent: etc
nav_order: 2
---

# React와 Vue

## 공통점

가상 DOM을 활용합니다.
반응적이고 조합 가능한 컴포넌트를 제공합니다.
코어 라이브러리에만 집중하고 있고 라우팅 및 전역 상태를 관리하는 컴패니언 라이브러리가 있습니다.
속도는 비슷하게 빠르다.

## 차이점

### 최적화 방식

둘은 최적화 방식이 다르다. React에서는 컴포넌트의 state가 변경되면, 해당 컴포넌트를 루트로 시작해 전체 자식 컴포넌트들을 다시 렌더링(re-render) 한다. 자식 컴포넌트의 불필요한 렌더링을 줄이려면 shouldComponentUpdate() 메소드를 사용해야 한다. shouldComponentUpdate()는 자식 컴포넌트의 렌더링이 부모 컴포넌트로부터 받은 props에 의해 결정되게 해준다. 아래는 shouldComponentUpdate() 메소드를 사용해 자식 컴포넌트의 렌더링 여부를 결정하는 예제.

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

하지만 Vue는 렌더링을 할 때 컴포넌트의 종속성이 자동으로 추적된다. 따라서 상태가 변경될 때 실제로 어떤 컴포넌트를 다시 렌더링해야 하는지를 정확하게 알고 있다. 어떻게 보면 Vue의 컴포넌트들은 React의 shouldComponentUpdate()의 메소드를 가지고 있고, 이 메소드가 알아서 동작하는 것과 비슷한 느낌이라고 할 수 있다. 즉 최적화되지 않은 Vue의 업데이트는 최적화되지 않은 React보다 훨씬 빠르다.

### JSX vs Template

#### JSX

React의 모든 컴포넌트는 JSX를 사용한 렌더 함수를 통해 표현된다. JSX를 사용하면 온전히 자바스크립트만을 이용해서 화면을 만들 수 있다. 따라서 같은 스코프 내에서 자바스크립트의 값을 직접적으로 가져다 쓸 수도(참조) 있다. Vue에서도 렌더링 함수와 JSX를 지원하지만 기본적으로는 템플릿을 사용한다. 템플릿은 HTML과 비슷하기 떄문에 러닝 커브가 낮고, HTML으로 작업한 기존 애플리케이션을 Vue로 이전하기가 쉽다.

## React.js의 특징

## Vue.js의 특징

Vue.js는 여러모로 하이브리드형 프레임워크입니다. Evan You 가 똑똑하게 두 프레임워크의 좋은 점을 따왔다고 할까요? Angular 의 양방향 바인딩과 React의 Virtual Dom 을 지원합니다. 게다가 Flux 의 저장소 패턴을 구현한 vuex 를 통해 리액트의 단방향 데이터 바인딩도 활용 가능합니다. jQuery 와 같이 cdn을 이용한 사용법까지 지원해서 유연성면에서는 가장 뛰어나며, 레거시 시스템을 점진적으로 개선할 때도 적합합니다. 제가 생각하는 vue.js 의 백미는 Single File Component. 즉, .vue파일입니다. 아래와 같은 구조로 마크업과 스크립트, 스타일이 한 파일에 모여있습니다.

```js
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

## References

* [Vue.js - Comparison with Other Frameworks](https://vuejs.org/v2/guide/comparison.html)
* [React.js - 성능 최적화](https://ko.reactjs.org/docs/optimizing-performance.html#shouldcomponentupdate-in-action)
* [아하 프론트 개발기(0)— Angular, React, Vue.js 프레임워크 선택하기](https://medium.com/aha-official/%EC%95%84%ED%95%98-%ED%94%84%EB%A1%A0%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EA%B8%B0-0-angular-react-vue-js-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC-%EC%84%A0%ED%83%9D-f797392118d0)
