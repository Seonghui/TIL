---
description: 둘 다 리액트에서 다루는 데이터이다.
---

# props와 state

## 1. props

* 변하지 않는 데이터를 처리할 때 사용. 부모 컴포넌트가 자식 컴포넌트에게 주는 값.
* 자식 컴포넌트는 `props` 를 받기만 하고 직접 수정 불가능.
* 디폴트값 설정 가능\(defaultProps\), 하지만 아직 정식으로 지원하지는 않음.

### 1.1 defaultProps

실수로 props를 빠뜨릴 때, 혹은 props 값이 없을 때 사용하는 값. 즉 기본값.

```javascript
import React, { Component } from 'react';

class MyName extends Component {
  static defaultProps = {
    name: '기본이름'
  }
  render() {
    return (
      <div>
        안녕하세요! 제 이름은 <b>{this.props.name}</b> 입니다.
      </div>
    );
  }
}

export default MyName;
```

## 2. state

* 유동적인 데이터. 컴포넌트 내부에서 선언하며 값 변경 가능. 초기값 설정이 필수.
* `state` 는 그냥 사용하지 말 것. `this.setState` 사용하기.
* `state` 를 변경하면 render 가 다시 작동된다. 새로운 `state` 와 함께...
* 리액트에서는 `state` 내부의 값을 직접적으로 수정하면 절대로 안 된다. 이를 **불변성 유지**라고 하는데, 보통 자바스크립트에서 사용하는 `push`, `pop`, `splice`, `unshift` 같은 내장함수는 배열 자체를 수정하게 되므로 사용하지 않는다. 그 대신, 기존의 배열에 기반해서 새 배열을 만들어내는 함수인 `concat`, `slice`, `map`, `filter` 같은 함수를 사용해야 한다.
* 리액트에서 불변성 유지가 중요한 이유는, 불변성을 유지해야 필요한 상황에서 리렌더링되도록 설계할 수 있고, 그렇게 해야 나중에 성능도 최적화할 수 있기 때문이다.

