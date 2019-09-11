---
layout: default
title: 리액트 기본 사용법
parent: react
nav_order: 2
---

# 리액트 기본 사용법

## 1. Webpack, Babel

* Webpack: 리액트 프로젝트를 만들게 되면서 컴포넌트를 여러가지 파일로 분리해서 저장하게 되는데, 이렇게 분리한 파일을 하나로 저장하기 위해서는 Webpack이라는 도구를 사용해야 한다.
* [Babel](https://babeljs.io/): JSX와 새로운 자바스크립트 문법\(ES6\)를 사용하기 위해서는 Babel이라는 도구를 사용해야 한다.

## 2. 기본 사용법

* React를 사용하려면 React 파일을 불러와야 한다.
* 컴포넌트는 함수와 클래스로 만들 수 있다. 아래 예제는 클래스로 만든 컴포넌트 예제이다.
* 클래스 형태로 만들어진 컴포넌트에는 무조건 render\(\) 함수가 있어야 한다.
* 그 내부에는 JSX를 return 해줘야 한다. \(JSX는 div로 감싸져 있다.\)
* 작성한 컴포넌트를 내보내야\(export\) 한다.
* 이렇게 작성한 코드는 지정한 곳\(기본은 index.html의 \#root\)에 렌더링된다.

```javascript
import React, { Component } from 'react';

class App extends Component {
  render() {
    return (
      <div>
        <h1>안녕하세요 리액트</h1>
      </div>
    );
  }
}

export default App;
```

