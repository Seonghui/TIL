LifeCycle API
===
이 API 는 컴포넌트가 브라우저에서 나타날때, 사라질때, 그리고 업데이트 될 때, 호출되는 API이다. 참고로 컴포넌트가 렌더링 된다고 해서 무조건 브라우저에서 다시 그려지는 것이 아니다. 일단 Virtual DOM에 그리고 실제로 변하는 곳이 브라우저의 실제 DOM에 반영된다.

## 컴포넌트는 언제 렌더링 될까?
1. props 바뀔 때
2. state 바뀔 때
3. 부모 컴포넌트가 렌더링 될 때
4. this.forceUpdate()함수를 통해 강제 렌더링

## 렌더링 순서는 어떻게 될까
![lifecycle](https://user-images.githubusercontent.com/16531837/44030694-8fce0b76-9f3c-11e8-8626-21652d7dd7ed.png)
constructor -> getDerivedStateFromProps -> render -> componentDidMount 순으로 렌더링이 진행된다.  
첫 화면 랜더링 후 props가 바뀔경우 getDerivedStateFromProps -> shouldComponentUpdate 과정을 거치며 false일 경우 종료, true일 경우 render -> getSnapshotBeforeUpdate -> componentDidUpdate 후 종료된다. state가 바뀔 경우에는 바로 shouldComponentUpdate 부터 라이프사이클이 진행된다.

# 1. 컴포넌트 초기 생성
* constructor
컴포넌트 생성자 함수. 컴포넌트가 새로 만들어질 때마다 이 함수가 호출된다.

* componentDidMount
컴포넌트가 화면에 나타나게 됐을 때 호출된다. D3, masonry처럼 DOM을 사용해야하는 외부 라이브러리 연동을 하거나, 해당 컴포넌트에서 필요로 하는 데이터를 요청하기 위해 ajax 요청을 하거나, DOM의 속성을 읽거나 직접 변경하는 작업을 진행한다.

# 2. 컴포넌트 업데이트
* getDerivedStateFromProps
props로 받아온 값을 state로 동기화하는 작업을 해줘야 하는 경우 사용된다. 

* shouldComponentUpdate
컴포넌트를 최적화하는 작업에서 매우 유용하게 사용됨. state가 변경되거나 부모 컴포넌트로부터 새로운 props를 전달받을 때 실행된다. 리액트는 이 메소드의 반환 값에 따라서 re-render를 할지에 대한 여부를 결정한다.  
기본적으로 shouldComponentUpdate 메소드는 true를 반환한다. 하지만 원하지 않을 경우 이 return value를 false로 만들 수도 있다.

* componentDidUpdate
컴포넌트에서 render() 를 호출하고난 다음에 발생. 

# 3. 컴포넌트 제거
* componentWillUnmount
여기서는 주로 등록했었던 이벤트를 제거한다.

## References

- [Medium 포스트](https://medium.com/@shlee1353/%EB%A6%AC%EC%95%A1%ED%8A%B8-v16-3-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EB%9D%BC%EC%9D%B4%ED%94%84-%EC%82%AC%EC%9D%B4%ED%81%B4-%EC%A0%95%EB%A6%AC-a3a65de60beb)