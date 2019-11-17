---
layout: default
title: 주요 렌더링 경로(Critical Rendering Path)
parent: etc
nav_order: 2
---

# 주요 렌더링 경로(Critical Rendering Path)

{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
{:toc}

---

![주요 렌더링 경로](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-01.png)

* 탭 내부에서 발생하는 모든 작업(웹 콘텐츠)을 처리.
* 주요 역할은 HTML과 CSS, 자바스크립트를 사용자와 상호작용할 수 있는 웹페이지로 변환하는 것.
* 렌더러 프로세스의 메인 스레드가 브라우저로 전송된 대부분의 코드를 처리하는데, 간혹 웹 워커나 서비스 워커를 사용하는 경우에는 워커 스레드가 자바스크립트의 코드 일부를 처리한다.
* 이 프로세스가 최대한 효율적으로 이루어지도록 만드는 것을 보통 최적화라고 한다.

## 렌더링 처리방법

![렌더링 처리방법](https://post-phinf.pstatic.net/MjAxNzA3MDJfMTEy/MDAxNDk4OTI1MjU4OTY3.OR6bSfoKWEl_T4gQ1RDjKZ7dimgljLViK9QRPe9uQRQg.3FOoFdCJQGOBFEErH1EXUtHKnUfTg0-kkmnToN3-LHsg.PNG/image_3480670191498924844279.png?type=w1200)

렌더링 처리를 대강 요약하자면 아래와 같다.

1. 서버에서 응답으로 받은 HTML 데이터를 파싱한다.
2. HTML을 파싱한 결과로 DOM Tree를 만든다.
3. 파싱하는 중 CSS 파일 링크를 만나면 CSS 파일을 요청해서 받아 CSSOM(CSS Object Model)을 만든다.
4. DOM Tree와 CSSOM이 모두 만들어지면 이 둘을 사용해 Render Tree를 만든다.
5. Render Tree에 있는 각각의 노드들이 화면의 어디에 어떻게 위치할 지를 계산하는 Layout과정을 거쳐서,
6. 화면에 실제 픽셀을 Paint한다.
7. 화면의 픽셀로 변환한다. (합성)

### 1. 서버에서 응답으로 받은 HTML 데이터를 파싱

```html
<html>
<head>
  <meta charset="utf-8">
  <link href="./style.css" rel="stylesheet">
</head>
<body>
    <p>Hello, <span>web performance</span> students</p>
    <div>
        <img src="./image.png">
    </div>
</body>
</html>
```

페이지를 이동하라는 메시지를 렌더러 프로세스가 받고 HTML 데이터를 수신하기 시작하면 렌더러 프로세스의 메인 스레드는 **문자열(HTML)을 파싱해서 DOM(document object model)으로 변환**하기 시작한다. HTML 문서를 DOM으로 파싱하는 방법은 HTML 표준에 정의되어 있다. 여기서 미디어 파일을 만나면 추가로 요청을 받아서 받아오고, 자바스크립트 파일을 만나면 해당 파일을 받아와서 실행할 때까지 파싱이 멈춘다. JavaScript는 DOM 구조 전체를 바꿀 수 있는 `document.write()` 메서드와 같은 것을 사용해 문서의 모양을 변경할 수 있기 때문이다.

여기서 말하는 DOM이란 무엇일까? 직역하자면 문서 객체 모델이다. 문서 객체란 `<html>`이나 `<body>` 같은 html의 문서들을 자바스크립트가 이용할 수 있는 객체로 만들면 그것을 문서 객체라고 한다. 조금 더 명확하게 말하자면, DOM은 넓은 의미로 브라우저가 HTML 페이지를 인식하는 방법을 의미한다.

### 2. DOM Tree를 생성

![DOM Tree를 생성](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-02.png)

웹 사이트는 일반적으로 이미지, CSS, JavaScript와 같은 외부 리소스를 사용한다. 이러한 파일은 네트워크나 캐시에서 로딩해야 한다. DOM을 구축하기 위해 파싱하는 동안 이런 리소스를 만날 때마다 메인 스레드가 하나하나 요청할 수도 있을 것이다. 하지만 속도를 높이기 위해 '프리로드(Preload) 스캐너'가 동시에 실행된다. HTML 문서에 `<img>` 또는 `<link>` 와 같은 태그가 있으면 프리로드 스캐너는 HTML 파서가 생성한 토큰을 확인하고 브라우저 프로세스의 네트워크 스레드에 요청을 보낸다.

자바스크립트는 파싱을 막을 수 있다. `<script>` 태그를 만나면 HTML 파서는 HTML 문서의 파싱을 일시 중지한 다음 자바스크립트 코드를 로딩하고 파싱해 실행해야 한다. 왜냐면 자바스크립트는 DOM 구조를 바꿀 수 있어 문서의 모양을 변경할 수 있기 때문이다. HTML 파싱을 재개하기 전에 HTML 파서는 자바스크립트의 실행이 끝나기를 기다려야 한다. 리소스가 어떻게 로딩하길 원하는지 브라우저에 힌트를 줄 수 있다. 예를 들면 script를 로딩할 때 async 속성이나 defer 속성이 있다.

자바스크립트 처리는 [자바스크립트 엔진](/TIL/docs/javascript/javascript-1/how-does-javascript-work.html)이 한다. 렌더링 엔진의 HTML 파서가 DOM 생성 프로세스를 하던 중 스크립트 태그를 만나면, 자바스크립트 코드를 실행시키기 위해 자바스크립트 엔진에게 제어권한을 넘겨 주게 된다. DOM 트리가 다 형성되지 않았는데 자바스크립트에서 해당 DOM을 조작하려고 하면 문제가 발생하기 때문에 `<script>` 태그는 html의 body 태그 제일 아래에 놓는 것이 좋다.

![DOM Tree를 생성](https://post-phinf.pstatic.net/MjAxNzA3MDNfMTkw/MDAxNDk5MDkzMDA1NTk4.uXch89MWIheItGp06GiL9CpICJqaJ-o_cZldjWrtljwg.9qNsHS5oXG0Pj2QZS8C7xuP_DEmiZXUaFjMCaJd4J4Ig.PNG/full-process.png?type=w1200)

브라우저는 읽어들인 HTML 바이트 데이터를 해당 파일에 지정된 인코딩(ex.UTF-8)에 따라 문자열로 바꾸게 된디. 바꾼 문자열을 다시 읽어서, HTML표준에 따라 문자열을 토큰Token으로 변환한다. 이미지에서와 같이 이 과정에서`<html>` 은 StartTag: html 로, `</html>` 은 EndTag: html 로 변환된다. 이렇게 만들어진 토큰들을 다시 노드로 바꾸는 과정을 거친다. StartTag: html 이 들어왔으면 html노드를 만들고 EndTag:html 을 만나기 전까지 들어오는 토큰들은 html노드의 자식 노드로 넣는 식으로 변환이 이루어지기 때문에, 과정이 끝나면 Tree모양의 DOM(Document Object Model)이 완성되게 된다.

### 3. CSS에서 CSSOM으로

HTML을 파싱하다가 CSS링크를 만나면, CSS파일을 요청해서 받아오게 된다. 받아온 CSS파일은 HTML을 파싱한 것과 유사한 과정을 거쳐서 역시 Tree형태의 CSSOM으로 만들어진다. CSS 파싱은 CSS 특성상 자식 노드들이 부모 노드의 특성을 계속해서 이어받는(cascading) 규칙이 추가된다는 것을 빼고는 HTML파싱과 동일하게 이루어진다. 이렇게 CSSOM을 구성하는 것이 끝나야, 비로소 이후의 Rendering 과정을 시작할 수 있다. (그래서 CSS는 rendering의 blocking 요소라고 한다.)

DOM만으로는 웹 페이지의 모양을 알 수 없다. CSS로 웹 페이지 요소의 모양을 결정할 수 있기 때문이다. 메인 스레드는 CSS를 파싱하고 각 DOM 노드에 해당되는 계산된 스타일(computed style)을 확정한다. 계산된 스타일은 CSS 선택자(selector)로 구분되는 요소에 적용될 스타일에 관한 정보이다. 개발자 도구의 computed 패널에서 이 정보를 볼 수 있다. CSS를 전혀 적용하지 않아도 DOM 노드에는 계산된 스타일이 적용되어 있다. (브라우저 기본 스타일이 있기 때문. [Chromium 소스 코드의 html.css 파일](https://cs.chromium.org/chromium/src/third_party/blink/renderer/core/html/resources/html.css) 참고.)

![CSS에서 CSSOM으로](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-03.png)

### 4. Render Tree 생성

CSSOM을 모두 만들었으면, DOM과 CSSOM를 합쳐서 Render Tree를 만든다. Render Tree는 DOM Tree에 있는 것들 중에서 화면에 실제로 '보이는' 친구들만으로 이루어진다. 만약 CSS에서 display: none 으로 설정하였다면, 그 노드(와 그 자식 노드 전부)는 Render Tree에 추가되지 않는 것이다. 마찬가지로 화면에 보이지 않는 `<head>` 태그 안의 내용들도 Render Tree에는 추가되지 않는다. 그래서 아래 이미지의 Render Tree에는 `<head>` 태그와, display속성이 none인 `<p>` 태그 하위의 `<span>` 태그가 사라진 것을 확인할 수 있다.

![Render Tree 생성](https://post-phinf.pstatic.net/MjAxNzA3MDRfMTUz/MDAxNDk5MDk4ODk0MjIz.SvlNpa9CTDpuHxOVrFlJqminaiJK9zWN0wN9uyXP9vog.voakcaq1COJE6GHchrTqcrQov3QllzhayTRXbB0Su30g.PNG/render-tree-construction.png?type=w1200)

Render Tree에는 사실 여러 가지가 포함되어 있다. Render Object Tree, Render Layer Tree 등등을 합쳐서 화면을 그리는 데에 필요한 모든 정보를 가지고 있는 Render Tree가 완성된다. ('등등'에는 Render Style Tree, InlineBox Tree같은 것들도 있다.)

![Render Tree 생성](https://post-phinf.pstatic.net/MjAxNzA3MDRfMjA4/MDAxNDk5MTMyNjA2NzY2.omsYuQxxpcuBTNlQxuqfeaWOPxVow6cvNmSQNSvLwyUg.7-uIJppReNL5w9NIhYkLiOVgmr4KeTKwPty4Znyl7-8g.PNG/the_compositing_forest.png?type=w1200)

Render Object Tree가 위에서 말했듯이 DOM Tree의 노드 중에서 화면에 보이는 것들만으로 이루어지는 트리이다. block, inline, image, text, table같은 요소들이 Render Object가 된다. DOM Tree에서 `<div>`는 Render Object Tree에 Block element로, `<span>`은 Inline element로 옮겨지는 것이다.

Render Object의 속성에 따라 필요한 경우 Render Layer가 만들어진다. 그리고 이 Render Layer중에서 GPU에서 처리되는 부분이 있으면 다시 Graphic Layer로 분리된다. 대표적으로는 다음과 같은 속성들이 쓰였을 때 Graphic Layer가 만들어지게 된다. ('하드웨어 가속'을 사용할 수 있게 되어 성능을 좋게 한다고 할 때가 이 때이다.)

* CSS 3D Transform(translate3d, preserve-3d 등)이나 perspective 속성이 적용된 경우
* `<video>` 또는 `<canvas>` 요소
* CSS3 애니메이션함수나 CSS 필터 함수를 사용하는 경우
* 자식 요소가 레이어로 구성된 경우
* z-index 값이 낮은 형제 요소가 레이어로 구성된 경우

만약 이런거 저런거 하나도 없이 `<div>` 하나에 width정도 속성만 있다고 하면 레이어는 기본으로 만들어지는 하나만 사용하게 된다.

### 5. 레이아웃

화면에 보이는 노드들만을 가지고 있는 Render Tree가 다 만들어지면, 이제 Render Tree에 있는 각각의 노드들이 화면의 어디에 위치할 지를 계산하는 Layout과정을 거친다. CSSOM에서 가져온 스타일 정보들로 이미 얘가 어떻게 생겨야 한다는 것은 모두 알고 있지만, 그래서 현재 보이는 뷰포트를 기준으로 실제로 놓으려면 얘가 어디에 가야하는 지는 계산을 또 해야하는 것이다. 여기에서 CSS box model이 쓰이며, position(relative, absolute, fixed..), width, height 등등 틀과 위치에 관련된 부분들이 계산됩니다. 여기서 메인 스레드는 DOM과 계산된 스타일을 훑어가며 레이아웃 트리를 만든다. 레이아웃 트리는 x, y 좌표, 박스 영역(bounding box)의 크기와 같은 정보를 가지고 있다.

만약 width: 50% 로 되어있는데 브라우저를 리사이즈한다고 하면, 보이는 요소들은 변함이 없으니 Render Tree는 그대로인 상태에서, layout단계만 다시 거쳐 위치를 계산해서 그리게 된다.

![레이아웃](https://post-phinf.pstatic.net/MjAxNzA3MDRfMjU1/MDAxNDk5MTQ4MDAyMzcy.lpehOnK3sqhkXDwL8yQ8TnBmZCXvi2CKZFxxMkTeE-gg.71B6TjHwvmGs-g0U44y-qDTKB1ZIMG03ZO0Jky1UG5og.GIF/resize_browser.gif?type=w1200)

> 알아두면 좋은 것 - 레이아웃 트리는 DOM 트리와 비슷한 구조일 수 있지만 웹 페이지에 보이는 요소에 관련된 정보만 가지고 있다. display: none 속성이 적용된 요소는 레이아웃 트리에 포함되지 않는다(그러나 visibility: hidden 속성이 적용된 요소는 레이아웃 트리에 포함된다). 이와 비슷하게 p::before{content:"Hi!} 속성과 같은 의사 클래스(pseudo class)의 콘텐츠는 DOM에는 포함되지 않지만 레이아웃 트리에는 포함된다.

![레이아웃](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-05.png)

### 6. 페인트

DOM, 스타일, 레이아웃을 가지고도 여전히 페이지를 렌더링할 수 없다. 그림을 하나 따라 그리려고 한다고 생각해 보자. 요소의 크기, 모양, 위치를 알더라도 어떤 순서로 그려야 할지 판단해야 한다. 예를 들어 어떤 요소에 z-index 속성이 적용되었다면 HTML에 작성된 순서로 요소를 그리면 잘못 렌더링된 화면이 나온다. 즉, DOM에 선언된 노드 순서와 페인트 순서는 많이 다를 수 있다. 페인트 단계에서 메인 스레드는 페인트 기록(paint record)을 생성하기 위해 레이아웃 트리를 순회한다. 페인트 기록은 '배경 먼저, 다음은 텍스트, 그리고 직사각형'과 같이 페인팅 과정을 기록한 것이다. (퍼포먼스탭에서 볼수있는듯?)

![페인트](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-09.png)

### 7. 합성

![합성](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-14.gif)

브라우저는 문서의 구조와 각 요소의 스타일, 요소의 기하학적 속성, 페인트 순서를 알고 있다. 브라우저는 이제 웹 페이지를 어떻게 그릴까? 이 정보를 화면의 픽셀로 변환하는 작업을 래스터화(rasterizing)라고 한다.

가장 단순한 래스터화는 아마 뷰포트 안쪽을 래스터하는 것일 것이다. 사용자가 웹 페이지를 스크롤하면 이미 래스터화한 프레임을 움직이고 나머지 빈 부분을 추가로 래스터화한다. 이 방식은 Chrome이 처음 출시되었을 때 래스터화한 방식이다. 그러나 최신 브라우저는 합성(compositing)이라는 보다 정교한 과정을 거친다.

합성은 웹 페이지의 각 부분을 레이어로 분리해 별도로 래스터화하고 컴포지터 스레드(compositor thread)라고 하는 별도의 스레드에서 웹 페이지로 합성하는 기술이다. 스크롤되었을 때 레이어는 이미 래스터화되어 있으므로 새 프레임을 합성하기만 하면 된다. 애니메이션 역시 레이어를 움직이고 합성하는 방식으로 만들 수 있다. 자세한 정보는 [여기](https://blog.logrocket.com/eliminate-content-repaints-with-the-new-layers-panel-in-chrome-e2c306d4d752) 클릭.)

Chrome 개발자 도구의 Layers 패널에서 웹 사이트가 어떻게 레이어로 나뉘어 있는지 볼 수 있다.

#### 추가. 여러 레이어로 나눠서 합성하기

이건 읽어도 뭔 소린지 1도 모르겠습니다. [본문](https://d2.naver.com/helloworld/5237120) 여러 레이어로 나누기부터 참고.

## refs

* [최신 브라우저의 내부 살펴보기](https://d2.naver.com/helloworld/2922312)
* [Inside look at modern web browser](https://developers.google.com/web/updates/2018/09/inside-browser-part1)
* [브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361)
* [How Browsers Work: Behind the scenes of modern web browsers](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)
* [브라우저는 웹페이지를 어떻게 그리나요? - Critical Rendering Path](https://m.post.naver.com/viewer/postView.nhn?volumeNo=8431285&memberNo=34176766)
* [주요 렌더링 경로](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/?hl=ko)
* [HTML Critical rendering path의 이해](https://blog.asamaru.net/2017/05/04/understanding-the-critical-rendering-path/)
* [Understanding the Critical Rendering Path](https://bitsofco.de/understanding-the-critical-rendering-path/)
