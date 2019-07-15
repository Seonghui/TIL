# lifecycle

## LifeCycle API

이 API 는 컴포넌트가 브라우저에서 나타날때, 사라질때, 그리고 업데이트 될 때, 호출되는 API이다. 참고로 컴포넌트가 렌더링 된다고 해서 무조건 브라우저에서 다시 그려지는 것이 아니다. 일단 Virtual DOM에 그리고 실제로 변하는 곳이 브라우저의 실제 DOM에 반영된다.

### 컴포넌트는 언제 렌더링 될까?

1. props 바뀔 때
2. state 바뀔 때
3. 부모 컴포넌트가 렌더링 될 때
4. this.forceUpdate\(\)함수를 통해 강제 렌더링

### 렌더링 순서는 어떻게 될까

![lifecycle](https://user-images.githubusercontent.com/16531837/44030694-8fce0b76-9f3c-11e8-8626-21652d7dd7ed.png)

* Mounting: 컴포넌트가 브라우저상에 나타난다는 것
* Updating: 컴포넌트의 props나 state가 바뀌었을 때
* Unmounting: 컴포넌트가 브라우저상에서 사라질 때

  constructor -&gt; getDerivedStateFromProps -&gt; render -&gt; componentDidMount 순으로 렌더링이 진행된다.  

  첫 화면 랜더링 후 props가 바뀔경우 getDerivedStateFromProps -&gt; shouldComponentUpdate 과정을 거치며 false일 경우 종료, true일 경우 render -&gt; getSnapshotBeforeUpdate -&gt; componentDidUpdate 후 종료된다. state가 바뀔 경우에는 바로 shouldComponentUpdate 부터 라이프사이클이 진행된다.

## 1. Mounting \(컴포넌트 초기 생성\)

* constructor

  ```javascript
  constructor(props) {
  super(props);
  }
  ```

  컴포넌트 생성자 함수. 컴포넌트가 새로 만들어질 때마다 이 함수가 호출된다. 가장 먼저 실행되는 함수. 주로 컴포넌트가 가지고 있을 state를 초기화. 아니면 컴포넌트가 만들어지기 전에 미리 해야할 작업을 정해줌.

* getDerivedStateFromProps

  ```javascript
  static getDerivedStateFromProps(nextProps, prevState) {
  // ...
  }
  ```

  props로 받은 값을 state에 동기화시키고 싶을 때. Mounting 과정에서도 실행되고 props가 Updating 될때도 실행된다. static 값으로 넣어주어야 한다.

* componentDidMount

  ```javascript
  componentDidMount() {
  // ...
  }
  ```

  컴포넌트가 화면에 나타나게 됐을 때 호출된다. D3, masonry처럼 DOM을 사용해야하는 외부 라이브러리 연동을 하거나, 해당 컴포넌트에서 필요로 하는 데이터를 요청하기 위해 ajax 요청을 하거나, DOM의 속성을 읽거나 직접 변경하는 작업을 진행한다.

## 2. Updating\(컴포넌트 업데이트\)

* shouldComponentUpdate

  ```javascript
  shouldComponentUpdate(nextProps, nextState) {
  // return false 하면 업데이트를 안 함
  return true;
  }
  ```

  컴포넌트를 최적화하는 작업에서 매우 유용하게 사용됨. state가 변경되거나 부모 컴포넌트로부터 새로운 props를 전달받을 때 실행된다. 리액트는 이 메소드의 반환 값에 따라서 re-render를 할지에 대한 여부를 결정한다.  
  기본적으로 shouldComponentUpdate 메소드는 true를 반환한다. 하지만 원하지 않을 경우 이 return value를 false로 만들 수도 있다.

* getSnapshotBeforeUpdate

  ```javascript
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // ...
  }
  ```

  렌더링을 하고나서 업데이트를 바로 하기 전에 스크롤의 위치, 아니면 해당 돔의 크기를 가져오고 싶다... 등등 할때 사용.

* componentDidUpdate

  ```javascript
  componentDidUpdate(prevProps, prevState, snapshot) {
  // ...
  }
  ```

  컴포넌트에서 render\(\) 를 호출하고난 다음에 발생. 예를 들어 state 가 변경되었으면 이전의 상태와 지금의 상태를 비교했을 때 만약 다르다면 작업을 해라... 등등 할때 사용.

## 3. Unmounting\(컴포넌트 제거\)

* componentWillUnmount

  ```javascript
  componentWillUnmount() {
  // ...
  }
  ```

  여기서는 주로 등록했었던 이벤트를 제거한다. 여기서 이벤트 등록은 주로 componentDidMount 에서 등록했던 이벤트임.

## 4. 기타 \(에러 발생\)

* componentDidCatch

  ```javascript
  componentDidCatch(error, info) {
  this.setState({
    error: true
  });
  }
  ```

  컴포넌트 자신의 render 함수에서 에러가 발생해버리는 것은 잡아낼 수는 없지만, 그 대신에 컴포넌트의 자식 컴포넌트 내부에서 발생하는 에러들을 잡아낼 수 있다. 즉 자식 컴포넌트의 에러를 부모 컴포넌트가 잡아낸다.

### References

* [Medium 포스트](https://medium.com/@shlee1353/%EB%A6%AC%EC%95%A1%ED%8A%B8-v16-3-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EB%9D%BC%EC%9D%B4%ED%94%84-%EC%82%AC%EC%9D%B4%ED%81%B4-%EC%A0%95%EB%A6%AC-a3a65de60beb)
* [누구든지 하는 리액트](https://react-anyone.vlpt.us/05.html)

