LifeCycle API
===
이 API 는 컴포넌트가 브라우저에서 나타날때, 사라질때, 그리고 업데이트 될 때, 호출되는 API이다.

# 1. 컴포넌트 초기 생성
* constructor
컴포넌트 생성자 함수. 컴포넌트가 새로 만들어질 때마다 이 함수가 호출된다.

* componentWillMount
컴포넌트가 화면에 나가기 직전에 호출되는 API. 요새는 잘 사용하지 않음.

* componentDidMount
컴포넌트가 화면에 나타나게 됐을 때 호출된다. D3, masonry처럼 DOM을 사용해야하는 외부 라이브러리 연동을 하거나, 해당 컴포넌트에서 필요로 하는 데이터를 요청하기 위해 ajax 요청을 하거나, DOM의 속성을 읽거나 직접 변경하는 작업을 진행한다.

# 2. 컴포넌트 업데이트
* componentWillReceiveProps
컴포넌트가 새로운 props를 받게됐을 때 호출된다. 이 안에서는 주로 state가 props에 따라 변해야하는 로직을 작성한다. 새로 받게 될 props는 nextProps으로 조회할 수 있다. (이때 this.props를 조회하면 업데이트 되기 전의 API이니 참고할 것. 하지만 v16.3 부터 deprecate 되었음.).

* shouldComponentUpdate
컴포넌트를 최적화하는 작업에서 매우 유용하게 사용됨. 

* componentDidUpdate
컴포넌트에서 render() 를 호출하고난 다음에 발생. 

# 3. 컴포넌트 제거
* componentWillUnmount
여기서는 주로 등록했었던 이벤트를 제거한다.