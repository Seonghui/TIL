# 브라우저 동작 원리

브라우저가 HTML을 해석하고 화면에 나타내는 방법은 HTML,CSS 표준에 따르게 되는데, 브라우저에 따라 스펙 정의가 좀 다를 수도 있음

## 구성요소

* UI : 주소바, 뒤로가기, 앞으로가기, 북마크 메뉴 버튼 등등
* 브라우저 엔진: 렌더링 엔진에 작업을 요청하고 다룸
* 렌더링 엔진: 요청된 컨텐츠를 회면에 표시하게 만들어주는 엔진. HTML과 CSS을 파싱하고 화면에 파싱된 콘텐츠를 표현
* 네트워킹: HTTP request와 같은 네트워크 호출을 위해 필요
* UI 백엔드: 콤보박스나 윈도우와 같은 기본 위젯을 화면에 그리는데 필요
* 자바스크립트 해석기: 자바스클비트 코드를 파싱하고 실행하는데 필요
* 데이터 스토리지

### 웹 엔진의 실행흐름

![](https://image.slidesharecdn.com/web-browserarchitecture-150609231155-lva1-app6892/95/web-browser-architecture-19-638.jpg?cb=1433891674)

#### 1. 불러오기\(Loading\)

* 웹페이지와 페이지에 속한 모든 리소스를 불러오는 과정

#### 2. 파싱\(Parsing\)

* DOM 트리를 만드는 과정 \(HTML 문서 -&gt; DOM 트리\)
* HTML, XML 파서 등등

#### 3. 렌더링 트리 생성\(Producing a render tree\)

![](https://cdn-images-1.medium.com/max/1200/1*Z32YHoZNEgAHCaozilfDIA.png)

* 파싱으로 생성된 DOM 트리는 HTML/XML 문서의 내용을 트리 형태로 구조화함. 이게 DOM 노드.
* 이때 head, title, body 태그와 css display 속성이 none인 태그는 렌더링 트리에 추가되지 않음

#### 4. CSS style 결정\(Style Resolution\)

* DOM 트리와는 별도의 render tree로 구성
* 모든 css 스타일을 분석하고 최종적으로 어떤 태그에 어떤 스타일 규칙이 적용되는지 결정

#### 5. 레이아웃\(Layout\)

* render tree가 생성된 이후 수행됨
* 렌더링 트리가 생성될 떄 렌더된 객체는 위치나 크기를 가지고 있지 않음
* 각 렌더 객체가 위치와 크기를 가지게 되는 과정을 레이아웃이라고 함

#### 6. 그리기\(Painting\)

* render tree를 탐색하면서 특정 메모리 공간에 RGB 값을 채우는 과정
* UI 백엔드를 이용

### 브라우저별 렌더링 엔진

* IE - Trident
* 크롬, 사파리: Webkit
* 파이어폭스: Gecko
* 오페라: Presto

### 브라우저별 자바스크립트 엔진

* IE: Chakra
* 크롬: V8
* 파이어폭스: JaegerMonkey
* 사파리: Nitro
* 오페라: Carakan

### 스크립트와 CSS 처리 순서

* HTML은 기본적으로 동기적 처리
* 스크립트가 실행될 떄 보통 파싱은 중지
* 파싱 중 스크립트가 CSS 정보 요청 가능
* CSS 처리 중일 때 관련 스크립트 실행 중지

