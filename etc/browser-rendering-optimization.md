# browser-rendering-optimization

### 프론트엔드 성능 최적화

* 서버 성능 분석가의 관심사: 서버가 얼마나 많은 요청을 처리할 수 있나? \(TPS: Transition Per Second\)
* FE 성능 분석가의 관심사: 사용자 입력에 얼마나 빠르게 반응할 수 있나? \(LAI: Loading And Interaction\)

## LAI \(Loading And Interaction\)

### 초기 로딩 속도

얼마나 빨리 페이지를 볼 수 있는가? 일단 waterfall 차트를 중점적으로 봐야함. 크롬 개발자 도구에서 네트워크 탭에 해당함. 로딩 속도는 아래와 같이 개선함.

#### 높이 줄이기 \(Request 수 줄이기\)

request 수를 줄여야함. 여러개를 불러오기보다는 하나에 뭉쳐서 한번에 불러오기. 제일 효과적인 건 이미지다.

ex\) 흩어져있는 js, css를 하나로 합치기. CSS Sprite. DATA URL\(캐싱되지 않아도 될 이미지를 URL에 포함\).

여기서 가장 효과적인 방법은 초기 로딩시 불필요한 자원은 삭제하거나 뒤로 이동\(Lazy\)시키는 거. 불필요한 자원의 예시는 실수로 요청한 자원이나 초기 로딩시 필요없는 JS, 뷰포트 바깥에 있는 이미지가 포함됨.

브라우저는 호스트당 동시에 연결되어있는 개수가 정해져있음. 다시 말해 한번에 사용할 수 잇는 커넥션 수가 정해져있어 요청 수가 많아지면 사용할 수 있는 커넥션 수를 초과하게 됨. 이렇게 초과하게 되는 경우 다른 요청은 브라우저에서 대기. 따라서 요청은 줄이면 줄일 수록 좋음.

#### 폭 줄이기 \(request 시간 줄이기\)

* TTFB \(Time to First Byte\)가 오래 걸리면 서버 비즈니스 로직이 느린 것
* Content Download가 오래 걸린다면 네트워크 속도나 콘텐츠의 크기가 큰 것 \(minify, gzip 등으로 압축을 하자. 이미지는 꼭 줄이자.\)
* 레티나 대응하는 경우 이미지\(2x, 3x... 보통 2x\)가 커지는데 picture, sourcse, srcset 태그를 사용하면 된다. 그런데 그렇게 현실적인 방법은 아님. \(왜지? 더 알아보자. mdn 반응형 이미지 찾아볼것\)

![ttfb](https://i.imgur.com/uxtPlkv.png)

#### 간격 땡기기: Request 계단 간격 땡기기

1. 서버로부터 HTML 문자열을 stream으로 받음
2. head 태그에 포함된 자원을 병렬로 다운로드
3. head 태그에 포함된 자원을 모두 실행
4. body 태그부터 화면을 그리기 시작
5. DOM 구성이 완료되면 DOMContentLoaded 이벤트 발생
6. 모든 자원의 로딩 완료되면 load 이벤트 발생
7. 따라서 head 먼저 실행 -&gt; dom 그림 -&gt; 콘텐츠\(이미지 등\) 불러오는 식.
8. head 태그에는 css와 필수 js만
9. js는 되도록이면 body 태그 마지막에 넣자
10. DOM 제어와 관련이 있는 스크립트는 defer, 의존성이 없는 스크립트는 async \(더 알아보기\)
11. css가 불려진 다음에 css에서 사용하는 폰트, 이미지가 로딩. 만약 css 링크에 preload 속성을 사용하는 경우 css와 함께 폰트, 이미지가 로딩.

#### 마지막으로 점검하기

* First Paint\(FP\): 헤드 태그 종료후
* First Meaningful Paint\(FMP\): 페이지의 주요 콘텐츠가 나타나는 시점임. 사이트마다 다른데 예시로 검색 엔진의 결과 헤드라인 텍스트가 있음.
* 각각의 request를 균등한 크기로 맞추기. 뭐 하나가 엄청 길게 보이고 그러면 안됨.

### 인터렉션 속도

스크롤 버벅, 키보드 입력 버벅, 애니메이션 동작 버벅이는 경우임. 브라우저의 메인 쓰레드를 괴롭히지 않음 됨. 문제는 자바스크립트인데, 자바스크립트가 돔을 건들면 기본적으로 메인 쓰레드에 의해 렌더링 파이프라인이 동작. 그냥 자바스크립트로 돔을 건드려서 애니메이션 효과주는건 자제하자.

#### 렌더링 파이프라인

![rendering pipeline](https://i.imgur.com/AfxbN4s.png)

자바스크립트로 돔 변경하기

1. style recalculate: DOM의 최종 스타일 계산
2. layout: DOM의 배치와 크기 계산
3. paint: 화면에 그리기
4. composite: 레이어 조합하기 \(CPU\)

웹페이지는 하나의 거대한 레이어로 되어있다. 여기서 GPU는 각각의 레이어를 합치는 작업을 한다. 여긴 전체적으로 다시 봐야할듯.

## 개선 작업은 어찌 이루어지나?

### 대상 선정하기

나무 말고 숲을 보자. 서비스에서 가장 많이 사용하는 화면이 무엇인가? 가장 가치있는 화면이 무엇인가? 기준은 세우기 나름. 사용자에게 꼭 보야주어야 하는 부분 위주로 세울 수도 있음. 예를 들면 First Meaningful Paint.

### 프로세스

측정 -&gt; 분석 -&gt; 최적화 순으로

### 언제까지?

목표에 도달할 때까지

## refs

* [https://www.slideshare.net/NaverEngineering/naver-tech-concertfe2019-fe](https://www.slideshare.net/NaverEngineering/naver-tech-concertfe2019-fe)

