# chrome-dev-tool

### 크롬 개발자도구로 웹사이트 퍼포먼스 분석하기

## Performance

* wasd 키로 타임라인 zoom 및 이동 가능

  **데이터 분석**

  **FPS Chart**

  ![FPS](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/fps-chart.svg?hl=ko)

* 빨간색으로 되어있는 것은 프레임 드랍이 심해 사용자 경험에 좋지 않은 거임

#### CPU Chart

![CPU](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/cpu-summary.svg?hl=ko)

* 하단에 있는 Summary 탭이랑 연결되어 있음
* 여기가 색으로 가득채워져있다는 건 CPU가 사용량이 초과되었음을 의미

#### Scrubbing

![Scrubbing](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/screenshot.png?hl=ko)

* FPS나 CPU 차트에 마우스 오버를 하면 해당 지점의 스크린샷을 볼 수 있음
* 마우스를 오른쪽이나 왼쪽으로 움직이면 녹화된 화면이 재생됨
* 이걸 Scrubbing이라고 부름

#### Frames

![Scrubbing](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/frame.png?hl=ko)

* 초록색 네모에 마우스를 가져다대면 특정 프레임의 FPS 확인 가능

### Performance 탭을 활용해 병목 현상 찾기

1. 아무것도 클릭하지 않은 상태에서 써머리를 보면 렌더링에서 엄청나게 시간을 잡아먹는걸 알 수 있음. 따라서 목표는 렌더링 시간 줄이기.

![Scrubbing](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/summary.svg?hl=ko)

1. 메인 섹션을 확대해서 보자.. x축은 시간에 따른 recording을 보여준다. 각각의 바는 이벤트인데, 만약 이 바가 길다는 것은 이벤트가 오래 걸렸다는 것을 의미함. y축은 콜스택이다. 이벤트들이 서로 겹쳐져있는 경우, 위쪽에 위치한 이벤트가 아래의 이벤트를 발생시켰다는 것을 의미한다.

![event](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/main.svg?hl=ko)

1. 확대해서 Animation Frame Fired 이벤트를 보면 우측 상단에 빨간 삼각형이 있는게 보임. 만약 이 삼각형이 있다면 병목현상의 문제가 아마 이 이벤트와 연관이 있을수도 있다는 것을 의미함. \(여기서의 Animation Frame Fired 이벤트는 requestAnimationFrame 콜백이 실행될 때마다 일어남\)

![Animation Frame Fired ](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/zoomed.png?hl=ko)

1. Animation Frame Fired 이벤트를 클릭하고 써머리 탭을 보면 이 이벤트에 대한 정보가 나옴. 여기서의 중요 포인트는 **reveal** 이라고 적혀있는 링크임. 이 링크를 클릭하면 Animation Frame Fired 이벤트를 실행한 이벤트를 강조해서 표시함. **app.js:94** 링크를 클릭하면 이 이벤트와 관련있는 소스코드로 이동함.

![summary](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/animation-frame-fired.png?hl=ko)

1. app.update 이벤트 아래로 보라색 이벤트들이 있는데, 이 보라색 이벤트를 클릭하고 써머리를 보면 관련 파일의 라인이 적혀있음.

![event](https://i.imgur.com/ILxri3L.png)

1. 이벤트 관련 파일 링크를 클릭하면 문제가 있는 파일의 라인으로 이동함. 바로 이 라인이 문제인 것.

![file](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/forced-layout-src.png?hl=ko)

이 예제의 문제점은 각 애니메이션 프레임에서 사각형의 스타일을 각각 변경한 다음 사각형의 위치를 각각 쿼리\(?\)하는 것임. 스타일이 바뀌면 브라우저는 사각형의 위치 변경 여부를 감지하지 못하기 때문에 위치를 계산하기 위해 사각형을 다시 re-layout 함

## refs

[https://developers.google.com/web/tools/chrome-devtools/](https://developers.google.com/web/tools/chrome-devtools/)

