자주 까먹는 각종 개념 정리
===

# 웹팩(Webpack)

![이미지](https://t1.daumcdn.net/cfile/tistory/9915694B5A33349630)


## 개념

모듈 번들러라고 함. 파일 간의 연관 관계를 파악해서 하나의 자바스크립트 파일로 변환해 주는 도구. 다시 말하자면 이곳저곳 퍼져있는 파일들을 하나의 자바스크립트 파일로 만들어 정리해주는 역할임. 열심히 코딩하고 웹팩의 이런 저런 설정 건드린 다음 빌드하면 됨.


## 왜 쓸까?

웹페이지가 화면에 나타날 때마다 css, js, 이미지 파일 등을 로드해야 하는데 서버로 HTTP 네트워크 요청을 보내서 이 파일들을 불러옴. 그런데 요청 숫자가 늘어나면 화면 로딩 시간이 길어지기 때문에 하나의 파일에 합쳐서 한번에 로드하려고 하는 거임. 이런 문제를 해결하기 위해 Gulp나 Grunt 같은 Task runner(주로 CSS, Javascript 파일을 minifying하는 자동화 툴)들이 등장했지만 기승전 웹팩이 됨. 웹팩이 빠르기도 하고 제공하는 기능이 많기 때문.


# 보일러플레이트(Boilerplate)

쉽게 말해 재사용 가능한 프로그램임. 만약 react.js라는 기술을 사용하려면 이런저런 것들을 설치하고 설정을 건드려야 되는데, 만약 이렇게 하나하나 구축하면 시간이 매우 오래걸림. 따라서 바로 시작할 수 있는 환경을 제공해주는 것이라고 생각하면 됨.
예를 들어 React.js의 경우, create-react-app을 사용하면 쉽게 react.js 개발환경을 구축할 수 있음.


# 패키지 관리자(Package Manager)

## npm (Node Package Manager)

자바스크립트 프로그래밍 언어를 위한 패키지 관리자. node.js로 만들어진 모듈이나 패키지 관리가 가능. 말도 안 되는 비유를 하자면 자바스크립트(node.js)계의 앱스토어(?)라고 생각하면 됨. 아무튼 기존의 모듈들을 사용해서 적절하게 활용하면 개발을 편하게 할 수 있음. (ex, react.js용 음력 달력 모듈, react.js용 파일 업로드 모듈 등)


## yarn

새로운 패키지 매니저임. npm보다 빠르고 안정적이고 보안성이 뛰어나다고 주장하고 있음. npm을 통해 설치할 수 있음.