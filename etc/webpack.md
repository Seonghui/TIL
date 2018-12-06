Webpack
===
> wepback 4 기준

# 주요 개념
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

## 1. mode
배포용, 개발용 등등 표기. 배포용일 경우 최적화가 알아서 됨.


**참고!!** Webpack이나 Browserify 같은 빌드 툴을 사용할 때, production mode는 `process.env.NODE_ENV`에 의해 결정됨. 기본적으로는 개발 모드임.


## 2. entry
파일을 읽어들이기 시작하는 부분. 


## 3. output
결과물이 어떻게 나올지 설정.
path는 output으로 나올 파일이 저장될 경로임.


## 4. loader
rules 프로퍼티 안에 정의됨. 웹팩으로 빌드할 때 html, css, 이미지 파일 등을 자바스크립트로 변환하기 위해 필요한 설정을 정의하는 속성임. vue의 경우 vue-loader를 정의하는 곳이기도 함.


## 5. plugin
https://webpack.js.org/concepts/plugins/
말그대로 웹팩 플러그인임. 웹팩으로 빌드하고 나온 결과물에 추가 기능을 제공.


## 6. resolve
웹팩으로 빌드할 때 해당 파일이 어떻게 해석되는지 정의하는 속성임. 예를 들어 뷰의 경우, 풀 버전의 라이브러리(vue.esm.js)를 선택할지 아니면 런타임 버전(vue.runtime.esm.js)을 설정하지를 정할 수 있음.


# How to run
## 1. dev
개발 서버는 node.js 서버로 `npm run dev` 명령어를 통해 실행할 수 있음. 개발 서버로 빌드한 결과물은 시스템에 존재하지 않고 컴퓨터 메모리에만 저장이 됨. 

## 2. build
`npm run build` 명령어를 사용해서 빌드. 


# vue webpack 템플릿 사용하고 삽질한 후기
* webpack 템플릿(simple 아님)을 다운로드할 경우 웹팩 설정 파일은 베이스/개발/배포용으로 나뉘어짐. 이번에 하면서 개발/배포용 파일은 건들 필요가 없었고 아직 뭐가 뭔지도 아직 모르겠음.

* 설치한 플러그인은 webpack.base.conf.js에 등록했음.

* 이외 config 폴더에서 또 이것저것 설정할 수 있는데, 제대로 알아보지는 않았지만 path 설정 같은 것이 대부분이었음. 이번에 개발모드에서 publicPath를 vue.js의 로컬서버 url로 등록하는 것도 config 폴더의 index.js 파일에서 설정함.

* 뭐쨌든 npm run build로 프로젝트 파일의 모든 static 파일을 모은다음 collectstatic으로 다시 모으고 그걸 장고 웹팩 로더를 사용해서 불러오는 방식임. 각 장고 앱별로 webpack을 설치한 다음 빌드해서 실행해도 아주 잘 작동함.