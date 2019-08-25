---
description: webpack 4 기준
---

# webpack

## 1. 주요 개념

![&#xC774;&#xBBF8;&#xC9C0;](https://t1.daumcdn.net/cfile/tistory/9915694B5A33349630)

모듈 번들러라고 함. 파일 간의 연관 관계를 파악해서 하나의 자바스크립트 파일로 변환해 주는 도구. 다시 말하자면 이곳저곳 퍼져있는 파일들을 하나의 자바스크립트 파일로 만들어 정리해주는 역할임. 열심히 코딩하고 웹팩의 이런 저런 설정 건드린 다음 빌드하면 됨.

### 왜 쓸까?

웹페이지가 화면에 나타날 때마다 css, js, 이미지 파일 등을 로드해야 하는데 서버로 HTTP 네트워크 요청을 보내서 이 파일들을 불러옴. 그런데 요청 숫자가 늘어나면 화면 로딩 시간이 길어지기 때문에 하나의 파일에 합쳐서 한번에 로드하려고 하는 거임. 이런 문제를 해결하기 위해 Gulp나 Grunt 같은 Task runner\(주로 CSS, Javascript 파일을 minifying하는 자동화 툴\)들이 등장했지만 기승전 웹팩이 됨. 웹팩이 빠르기도 하고 제공하는 기능이 많기 때문.

## 2. 예시

```javascript
const webpack = require('webpack');
module.exports = {
  mode: 'development',
  entry: {
    app: '',
  },
  output: {
    path: '',
    filename: '',
    publicPath: '',
  },
  module: {
    rules: [
      ...
    ]
  },
  resolve: {
    modules: ['node_modules'],
    extensions: ['.js', '.json', '.jsx', '.css'],
  },
};
```

### 1. mode

배포용, 개발용 등등 표기. 배포용일 경우 최적화가 알아서 됨.

**참고!!** Webpack이나 Browserify 같은 빌드 툴을 사용할 때, production mode는 `process.env.NODE_ENV`에 의해 결정됨. 기본적으로는 개발 모드임.

### 2. entry

파일을 읽어들이기 시작하는 부분.

### 3. output

결과물이 어떻게 나올지 설정. path는 output으로 나올 파일이 저장될 경로임.

### 4. loader

rules 프로퍼티 안에 정의됨. 웹팩으로 빌드할 때 html, css, 이미지 파일 등을 자바스크립트로 변환하기 위해 필요한 설정을 정의하는 속성임. vue의 경우 vue-loader를 정의하는 곳이기도 함.

### 5. plugin

[https://webpack.js.org/concepts/plugins/](https://webpack.js.org/concepts/plugins/) 말그대로 웹팩 플러그인임. 웹팩으로 빌드하고 나온 결과물에 추가 기능을 제공.

### 6. resolve

웹팩으로 빌드할 때 해당 파일이 어떻게 해석되는지 정의하는 속성임. 예를 들어 뷰의 경우, 풀 버전의 라이브러리\(vue.esm.js\)를 선택할지 아니면 런타임 버전\(vue.runtime.esm.js\)을 설정하지를 정할 수 있음.

## 3. How to run

### 1. dev

개발 서버는 node.js 서버로 `npm run dev` 명령어를 통해 실행할 수 있음. 개발 서버로 빌드한 결과물은 시스템에 존재하지 않고 컴퓨터 메모리에만 저장이 됨.

### 2. build

`npm run build` 명령어를 사용해서 빌드.

