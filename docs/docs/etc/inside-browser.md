---
layout: default
title: 브라우저 내부 살펴보기
parent: etc
nav_order: 2
---

# 브라우저 내부 살펴보기

브라우저가 HTML을 해석하고 화면에 나타내는 방법은 HTML,CSS 표준에 따르게 되는데, 브라우저에 따라 스펙 정의가 좀 다를 수도 있음

## 시작하기 전 용어 설명

컴퓨터나 스마트폰 애플리케이션을 실행할 때 애플리케이션을 구동하는 것이 바로 CPU와 GPU다.

* CPU: 컴퓨터의 두뇌
* GPU: 간단한 작업에만 특화되어 잇지만 여러 GPU 코어가 동시에 작업을 수행할 수 있음. 그래픽 작업 처리를 위해 개발되었다.
* 프로세스: 애플리케이션이 실행하는 프로그램
* 스레드: 프로세스 내부에 있으며 프로세스로 실행되는 프로그램의 일부

브라우저는 프로세스와 스레드를 어떻게 사용할까? 스레드를 많이 사용하는 프로세스 하나만 사용할 수도 있고, 스레드를 조금만 사용하는 프로세스를 여러 개 만들어 IPC로 통신할 수도 있다.

## 브라우저의 구성요소

* 유저 인터페이스\(UI\) : 주소바, 뒤로가기, 앞으로가기, 북마크 메뉴 버튼 등등
* 브라우저 엔진: 렌더링 엔진에 작업을 요청하고 다룸
* 렌더링 엔진: 요청된 컨텐츠를 회면에 표시하게 만들어주는 엔진. HTML과 CSS을 파싱하고 화면에 파싱된 콘텐츠를 표현. 파이어폭스는 모질라에서 직접 만든 게코\(Gecko\) 엔진을 사용하고 사파리와 크롬은 웹킷\(Webkit\) 엔진을 사용한다.
* 네트워킹: HTTP request와 같은 네트워크 호출을 위해 필요
* UI 백엔드: 콤보박스나 윈도우와 같은 기본 위젯을 화면에 그리는데 필요
* 자바스크립트 해석기: 자바스크립트 코드를 파싱하고 실행하는데 필요
* 데이터 스토리지: persistence layer. 로컬에만 저장한다. \(예를 들면 쿠키, 로컬스토리지 등\)

![&#xBE0C;&#xB77C;&#xC6B0;&#xC800;&#xC758; &#xAD6C;&#xC131;&#xC694;&#xC18C;](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/layers.png)

여기서 중요한 점은, 크롬 같은 브라우저는 렌더링 엔진의 여러 인스턴스 각 탭마다 하나씩 실행한다는 점이다. 각 탭은 별도의 프로세스에서 실행된다. 가장 간단한 예로 탭마다 렌더러 프로세스를 하나 사용하는 경우를 생각해 보자. 3개의 탭이 열려 있고 각 탭은 독립적인 렌더러 프로세스에 의해 실행된다. 이때 한 탭이 응답하지 않으면 그 탭만 닫고 실행 중인 다른 탭으로 이동할 수 있다. 만약 모든 탭이 하나의 프로세스에서 실행 중이었다면 탭이 하나만 응답하지 않아도 모든 탭이 응답하지 못하게 된다. 그리고 프로세스 개수가 한도에 다다르면 동일한 사이트를 열고 있는 여러 탭을 하나의 프로세스에서 처리한다.

## 브라우저 아키텍쳐 \(Chromium 기준\)

![&#xBE0C;&#xB77C;&#xC6B0;&#xC800; &#xC544;&#xD0A4;&#xD14D;&#xCCD0;](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_1-08.png)

1. 브라우저 프로세스: 주소 표시줄, 북마크 막대, 뒤로 가기 버튼, 앞으로 가기 버튼 등을 제어한다. 네트워크 요청이나 파일 접근과 같이 눈에 보이지는 않지만 권한이 필요한 부분도 처리한다.
2. 렌더러 프로세스: 탭 안에서 웹 사이트가 표시되는 부분의 모든 것을 제어한다. 예전 렌더러 프로세스는 여러 개가 만들어져 각 탭마다 할당되었지만, 지금은 사이트마다 프로세스를 할당한다.
3. 플러그인 프로세스: 웹 사이트에서 사용하는 플러그인\(예: Flash\)을 제어한다.
4. GPU 프로세스: GPU 작업을 다른 프로세스와 격리해서 처리한다. GPU는 여러 애플리케이션의 요청을 처리하고 같은 화면에 요청받은 내용을 그리기 때문에 GPU 프로세스는 별도 프로세스로 분리되어 있다.
5. 기타 프로세스: 확장 프로그램 프로세스, 유틸리티 프로세스 등의 프로세스가 있음.

![&#xBE0C;&#xB77C;&#xC6B0;&#xC800; &#xC544;&#xD0A4;&#xD14D;&#xCCD0;](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_1-09.png)

## 브라우저 프로세스

* 브라우저 프로세스는 탭 영역 밖에 있는 모든 부분을 제어한다.
* 브라우저의 주소 표시줄에 URL을 입력하면 브라우저가 인터넷에서 데이터를 가져와서 페이지를 표시한다. 간단한 이 동작에서 사용자가 사이트를 요청하고 브라우저가 페이지 렌더링을 준비하는 과정\(이 글에서는 이 과정을 '내비게이션'이라고 하겠다\)에 초점을 맞춰 살펴보겠다.

### 브라우저 프로세스의 구성요소 \(스레드\)

다른 것들도 있지만 본문은 세가지만 언급했음

* UI 스레드: 브라우저의 버튼과 입력란을 그림. 렌더러 프로세스를 먼저 찾거나 네트워크 요청과 동시에 렌더러 프로세스를 시작한다.
* 네트워크 스레드: 인터넷에서 데이터를 가져오기 위한 스택을 다룸
* 스토리지 스레드: 파일에 대한 접근을 제어

![&#xBE0C;&#xB77C;&#xC6B0;&#xC800; &#xD504;&#xB85C;&#xC138;&#xC2A4;&#xC758; &#xAD6C;&#xC131;&#xC694;&#xC18C;](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-01.png)

### 내비게이션 처리방법

내비게이션 처리를 대강 요약하자면 아래와 같다.

1. 입력 처리: 주소 표시줄에 URL 입력. UI 스레드가 검색어인지 쿼리인지 판별.
2. 내비게이션 시작: URL 입력하고 엔터키 누르면서 시작. UI 스레드가 네트워크 호출 시작.
3. 응답 읽기: 호출하고 나서 응답 본문이 들어오면 그 응답을 읽음
4. 렌더러 프로세스 찾기: 응답을 읽고 각 형식에 따라 적당한 렌더러 프로세스를 찾음 \(HTML이면 렌더러 프로세스, ZIP이면 다운로드 매니저\)
5. 내비게이션 실행: 데이터와 렌더러 프로세스가 전부 준비된 상태. 문서 로딩하면서 브라우저 프로세스도 업데이트 \(주소 표시줄, 뒤로 가기 버튼 등등\)
6. 로드 완료: 렌더러 프로세스는 계속 리소스를 로딩하고 페이지 렌더링. 끝내면 브라우저 프로세스로 IPC 메시지를 보냄 

> 여기서 IPC 메시지란 프로세스간 통신\(Inter-Process Communication, IPC\) 메시지를 의미한다.

#### 1. 입력 처리

![&#xC785;&#xB825; &#xCC98;&#xB9AC;](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-02.png)

사용자가 주소 표시줄에 타이핑을 시작하면 UI 스레드는 먼저 '입력되는 내용이 검색어\(search query\)인지 URL인지' 확인한다. Chrome에서 주소 표시줄은 검색창이기도 하다. UI 스레드는 입력되는 내용을 파싱해서 검색 엔진으로 이동할지 요청한 사이트로 이동할지 결정해야 한다.

#### 2. 내비게이션 시작

![&#xB0B4;&#xBE44;&#xAC8C;&#xC774;&#xC158; &#xC2DC;&#xC791;](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-03.png)

사용자가 Enter 키를 누르면 사이트의 콘텐츠를 가져오기 위해 UI 스레드가 네트워크 호출을 시작한다. 로딩 스피너가 탭의 모서리에 표시되고, 네트워크 스레드는 요청에 대한 DNS Lookup 및 TLS 연결 설정과 같은 적절한 프로토콜을 거쳐 요청을 처리한다. 이때 네트워크 스레드가 HTTP 301과 같은 서버 리디렉션 헤더를 수신할 수도 있다. 그런 경우에는 네트워크 스레드가 UI 스레드와 통신해 서버가 리디렉션을 요청했다는 것을 알린다. 그런 다음 새로운 URL 요청이 시작된다.

#### 3. 응답 읽기

![&#xC751;&#xB2F5; &#xC77D;&#xAE30; 1](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-04.png)

응답 본문인 페이로드가 들어오기 시작하면 네트워크 스레드는 필요에 따라 스트림의 처음 몇 바이트를 확인한다. 페이로드가 어떤 형식의 데이터인지는 응답 헤더의 Content-Type 헤더가 알려 주지만 정보가 없거나 잘못된 정보가 있을 수 있다. 그래서 이때 MIME 스니핑을 실행해 데이터의 실제 형식을 알아낸다. Chromium 소스 코드의 주석에 적힌 것처럼 데이터의 실제 형식을 알아내는 것은 '까다로운 작업'\(tricky business\)이다. 이 주석을 보면 브라우저가 얼마나 다양한 방법으로 Content-Type 헤더와 페이로드를 처리하는지 알 수 있을 것이다.

이 단계는 또한 Safe Browsing의 검사가 실행되는 단계이다. 도메인과 응답 데이터가 악성 사이트로 알려진 사이트와 일치하는 것 같다면 네트워크 스레드는 경고 페이지를 표시하라고 알린다. 이에 더해서 CORB\(Cross-Origin Read Blocking\) 기능이 서로 다른 사이트\(cross-site\)의 민감한 데이터가 렌더러 프로세스에서 실행되지 않게 검사한다.

![&#xC751;&#xB2F5; &#xC77D;&#xAE30; 2](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-05.png)

#### 4. 렌더러 프로세스 찾기

![&#xB80C;&#xB354;&#xB7EC; &#xD504;&#xB85C;&#xC138;&#xC2A4; &#xCC3E;&#xAE30;](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-06.png)

모든 검사가 끝나고 브라우저가 요청된 사이트로 이동해야 한다고 네트워크 스레드가 확신하게 되면 네트워크 스레드는 UI 스레드에 데이터가 준비되었음을 알린다. 그러면 UI 스레드는 웹 페이지의 렌더링을 수행할 렌더러 프로세스를 찾는다. 네트워크 요청이 응답을 받기까지 수백 밀리초가 걸릴 수 있기 때문에 이 과정을 더 빨리 진행하기 위한 최적화가 적용되어 있다. 2단계에서 UI 스레드가 네트워크 스레드로 URL 요청을 보낼 때 UI 스레드는 이미 어느 사이트로 이동할지 알고 있다. UI 스레드는 렌더러 프로세스를 먼저 찾거나 네트워크 요청과 동시에 렌더러 프로세스를 시작한다. 이런 방식에서는 모든 것이 예상대로 잘 진행된다면 네트워크 스레드가 데이터를 받을 때 이미 렌더러 프로세스는 준비 상태에 있게 된다. 만약 다른 사이트로 리디렉션이 이루어져 다른 프로세스가 필요하게 되면 미리 준비한 프로세스가 사용되지 않을 수도 있다.

#### 5. 내비게이션 실행

![&#xB0B4;&#xBE44;&#xAC8C;&#xC774;&#xC158; &#xC2E4;&#xD589;](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-07.png)

이제 데이터와 렌더러 프로세스가 준비되었으므로 내비게이션을 실행하도록 브라우저 프로세스에서 렌더러 프로세스로 IPC 메시지를 전송한다. 또한 렌더러 프로세스가 HTML 데이터를 계속 수신할 수 있도록 브라우저 프로세스는 데이터 스트림을 전달한다. 렌더러 프로세스에서 내비게이션이 실행되었다는 것을 브라우저 프로세스가 확인하고 나면 내비게이션이 완료되고 문서 로딩 단계가 시작된다.

이 시점에 주소 표시줄이 업데이트되고 보안 표시와 사이트 설정 UI도 새 페이지의 사이트 정보를 반영해 갱신된다. 탭에 대한 세션 기록이 업데이트되어 뒤로 가기 버튼과 앞으로 가기 버튼도 방금 이동한 사이트를 반영해 작동한다. 탭이나 창을 닫은 이후 탭과 세션을 복원할 수 있게 세션 기록이 디스크 드라이브에 저장된다.

#### 6. 로드 완료

![&#xB85C;&#xB4DC; &#xC644;&#xB8CC;](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-08.png)

내비게이션이 실행되면 렌더러 프로세스는 계속 리소스를 로딩하고 페이지를 렌더링한다. 이 단계에서 일어나는 일은 다음 글에서 자세하게 다루겠다. 렌더러 프로세스가 렌더링을 '끝내면' 브라우저 프로세스로 IPC 메시지를 보낸다\(이 시점은 페이지의 모든 프레임에서 onload 이벤트의 실행까지 끝낸 이후이다\). 그러면 UI 스레드는 탭에서 로딩 스피너의 작동을 중지한다.

'끝낸다\(finish\)'라고 표현한 이유는 클라이언트 사이드의 JavaScript가 여전히 추가적인 리소스를 로드하거나 이후에 새로운 뷰를 렌더링할 수도 있기 때문이다.

#### 추가 1. 다른 사이트로 내비게이션

![&#xB2E4;&#xB978; &#xC0AC;&#xC774;&#xD2B8;&#xB85C; &#xB0B4;&#xBE44;&#xAC8C;&#xC774;&#xC158; 1](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-09.png)

간단한 내비게이션이 완료되었다. 그런데 사용자가 주소 표시줄에 다른 URL을 다시 입력하면 어떻게 될까? 브라우저 프로세스는 동일한 단계를 거쳐 다른 사이트로 이동을 처리한다. 하지만 그전에 현재 렌더링된 사이트에서 `beforeunload` 이벤트를 확인해야 한다. `beforeunload` 이벤트는 탭을 닫거나 이동하려고 할 때 "이 사이트를 떠나시겠습니까?"라는 경고창을 만들 수 있다. JavaScript 코드를 포함해 탭 안의 모든 것은 렌더러 프로세스에 의해 처리되므로 브라우저 프로세스는 새로운 내비게이션 요청이 들어오면 현재 렌더러 프로세스를 확인해야 한다. \(내비게이션 요청은 렌더러 프로세스-&gt;브라우저 프로세스로 넘어간다\)

![&#xB2E4;&#xB978; &#xC0AC;&#xC774;&#xD2B8;&#xB85C; &#xB0B4;&#xBE44;&#xAC8C;&#xC774;&#xC158; 2](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-10.png)

#### 추가 2. 서비스 워커

![&#xC11C;&#xBE44;&#xC2A4; &#xC6CC;&#xCEE4;](https://d2.naver.com/content/images/2019/03/helloworld-201903-sangwoo-ko_2-12.png)

* 서비스 워커는 애플리케이션의 코드에 네트워크 프락시를 작성할 수 있는 수단이다.
* 서비스 워커를 통해 웹 개발자는 무엇을 로컬 캐시에 저장할지, 언제 네트워크에서 새 데이터를 가져올지 제어할 수 있다.
* 서비스 워커가 캐시에서 페이지를 로드하도록 설정되었다면 네트워크에서 데이터를 가져오도록 요청할 필요가 없다.
* 기억해야 할 중요한 점은 서비스 워커가 렌더러 프로세스에서 실행되는 JavaScript 코드라는 점이다. 
* 서비스 워커가 등록되면 서비스 워커의 범위는 참조로 유지된다. 내비게이션이 발생하면 네트워크 스레드는 도메인을 등록된 서비스 워커의 범위와 비교한다. 해당 URL에 등록된 서비스 워커가 있으면 UI 스레드는 서비스 워커 코드를 실행하기 위해 렌더러 프로세스를 찾는다. 서비스 워커는 네트워크에 데이터를 요청하지 않고 캐시에서 데이터를 가져올 수 있다. 또는 네트워크에 새 리소스를 요청할 수도 있다.
* 웹페이지와는 별개로 작동한다. 예를 들어 푸시 알림, 백그라운드 동기화와 같은 기능이 바로 서비스 워커인데 최신 기술이므로 Browser compatibility 확인하자.. 그리고 더 알고싶으면 [문서](https://developers.google.com/web/fundamentals/primers/service-workers/?hl=ko) 읽어보기

## 2. 렌더러 프로세스

> 중요 렌더링 경로Critical Rendering Path\)라고도 한다.

![&#xB80C;&#xB354;&#xB7EC; &#xD504;&#xB85C;&#xC138;&#xC2A4;](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-01.png)

* 탭 내부에서 발생하는 모든 작업\(웹 콘텐츠\)을 처리.
* 주요 역할은 HTML과 CSS, 자바스크립트를 사용자와 상호작용할 수 있는 웹페이지로 변환하는 것.
* 렌더러 프로세스의 메인 스레드가 브라우저로 전송된 대부분의 코드를 처리하는데, 간혹 웹 워커나 서비스 워커를 사용하는 경우에는 워커 스레드가 자바스크립트의 코드 일부를 처리한다.
* 이 프로세스가 최대한 효율적으로 이루어지도록 만드는 것을 보통 최적화라고 한다.

### 렌더링 처리방법

![&#xB80C;&#xB354;&#xB9C1; &#xCC98;&#xB9AC;&#xBC29;&#xBC95;](https://post-phinf.pstatic.net/MjAxNzA3MDJfMTEy/MDAxNDk4OTI1MjU4OTY3.OR6bSfoKWEl_T4gQ1RDjKZ7dimgljLViK9QRPe9uQRQg.3FOoFdCJQGOBFEErH1EXUtHKnUfTg0-kkmnToN3-LHsg.PNG/image_3480670191498924844279.png?type=w1200)

렌더링 처리를 대강 요약하자면 아래와 같다.

1. 서버에서 응답으로 받은 HTML 데이터를 파싱한다.
2. HTML을 파싱한 결과로 DOM Tree를 만든다.
3. 파싱하는 중 CSS 파일 링크를 만나면 CSS 파일을 요청해서 받아 CSSOM\(CSS Object Model\)을 만든다.
4. DOM Tree와 CSSOM이 모두 만들어지면 이 둘을 사용해 Render Tree를 만든다.
5. Render Tree에 있는 각각의 노드들이 화면의 어디에 어떻게 위치할 지를 계산하는 Layout과정을 거쳐서,
6. 화면에 실제 픽셀을 Paint한다.
7. 화면의 픽셀로 변환한다. \(합성\)

#### 1. 서버에서 응답으로 받은 HTML 데이터를 파싱한다

```markup
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

페이지를 이동하라는 메시지를 렌더러 프로세스가 받고 HTML 데이터를 수신하기 시작하면 렌더러 프로세스의 메인 스레드는 문자열\(HTML\)을 파싱해서 DOM\(document object model\)으로 변환하기 시작한다. HTML 문서를 DOM으로 파싱하는 방법은 HTML 표준에 정의되어 있다. 여기서 미디어 파일을 만나면 추가로 요청을 받아서 받아오고, 자바스크립트 파일을 만나면 해당 파일을 받아와서 실행할 때까지 파싱이 멈춘다. JavaScript는 DOM 구조 전체를 바꿀 수 있는 `document.write()` 메서드와 같은 것을 사용해 문서의 모양을 변경할 수 있기 때문이다.

#### 2. DOM Tree를 생성

![DOM Tree&#xB97C; &#xC0DD;&#xC131; 1](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-02.png)

웹 사이트는 일반적으로 이미지, CSS, JavaScript와 같은 외부 리소스를 사용한다. 이러한 파일은 네트워크나 캐시에서 로딩해야 한다. DOM을 구축하기 위해 파싱하는 동안 이런 리소스를 만날 때마다 메인 스레드가 하나하나 요청할 수도 있을 것이다. 하지만 속도를 높이기 위해 '프리로드\(Preload\) 스캐너'가 동시에 실행된다. HTML 문서에 `<img>` 또는 `<link>` 와 같은 태그가 있으면 프리로드 스캐너는 HTML 파서가 생성한 토큰을 확인하고 브라우저 프로세스의 네트워크 스레드에 요청을 보낸다. 자바스크립트는 파싱을 막을 수 있다. `<script>` 태그를 만나면 HTML 파서는 HTML 문서의 파싱을 일시 중지한 다음 자바스크립트 코드를 로딩하고 파싱해 실행해야 한다. 왜냐면 자바스크립트는 DOM 구조를 바꿀 수 있어 문서의 모양을 변경할 수 있기 때문이다. HTML 파싱을 재개하기 전에 HTML 파서는 자바스크립트의 실행이 끝나기를 기다려야 한다. 리소스가 어떻게 로딩하길 원하는지 브라우저에 힌트를 줄 수 있다. 예를 들면 script를 로딩할 때 async 속성이나 defer 속성이 있다.

![DOM Tree&#xB97C; &#xC0DD;&#xC131; 2](https://post-phinf.pstatic.net/MjAxNzA3MDNfMTkw/MDAxNDk5MDkzMDA1NTk4.uXch89MWIheItGp06GiL9CpICJqaJ-o_cZldjWrtljwg.9qNsHS5oXG0Pj2QZS8C7xuP_DEmiZXUaFjMCaJd4J4Ig.PNG/full-process.png?type=w1200)

브라우저는 읽어들인 HTML 바이트 데이터를 해당 파일에 지정된 인코딩\(ex.UTF-8\)에 따라 문자열로 바꾸게 된디. 바꾼 문자열을 다시 읽어서, HTML표준에 따라 문자열을 토큰Token으로 변환한다. 이미지에서와 같이 이 과정에서`<html>` 은 StartTag: html 로, `</html>` 은 EndTag: html 로 변환된다. 이렇게 만들어진 토큰들을 다시 노드로 바꾸는 과정을 거친다. StartTag: html 이 들어왔으면 html노드를 만들고 EndTag:html 을 만나기 전까지 들어오는 토큰들은 html노드의 자식 노드로 넣는 식으로 변환이 이루어지기 때문에, 과정이 끝나면 Tree모양의 DOM\(Document Object Model\)이 완성되게 된다.

#### 3. CSS에서 CSSOM으로

HTML을 파싱하다가 CSS링크를 만나면, CSS파일을 요청해서 받아오게 된다. 받아온 CSS파일은 HTML을 파싱한 것과 유사한 과정을 거쳐서 역시 Tree형태의 CSSOM으로 만들어진다. CSS 파싱은 CSS 특성상 자식 노드들이 부모 노드의 특성을 계속해서 이어받는\(cascading\) 규칙이 추가된다는 것을 빼고는 HTML파싱과 동일하게 이루어진다. 이렇게 CSSOM을 구성하는 것이 끝나야, 비로소 이후의 Rendering 과정을 시작할 수 있다. \(그래서 CSS는 rendering의 blocking 요소라고 합니다.\)

DOM만으로는 웹 페이지의 모양을 알 수 없다. CSS로 웹 페이지 요소의 모양을 결정할 수 있기 때문이다. 메인 스레드는 CSS를 파싱하고 각 DOM 노드에 해당되는 계산된 스타일\(computed style\)을 확정한다. 계산된 스타일은 CSS 선택자\(selector\)로 구분되는 요소에 적용될 스타일에 관한 정보이다. 개발자 도구의 computed 패널에서 이 정보를 볼 수 있다. CSS를 전혀 적용하지 않아도 DOM 노드에는 계산된 스타일이 적용되어 있다. \(브라우저 기본 스타일이 있기 때문. [Chromium 소스 코드의 html.css 파일](https://cs.chromium.org/chromium/src/third_party/blink/renderer/core/html/resources/html.css) 참고.\)

![CSS&#xC5D0;&#xC11C; CSSOM&#xC73C;&#xB85C;](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-03.png)

#### 4. Render Tree 생성

CSSOM을 모두 만들었으면, DOM과 CSSOM를 합쳐서 Render Tree를 만든다. Render Tree는 DOM Tree에 있는 것들 중에서 화면에 실제로 '보이는' 친구들만으로 이루어진다. 만약 CSS에서 display: none 으로 설정하였다면, 그 노드\(와 그 자식 노드 전부\)는 Render Tree에 추가되지 않는 것이다. 마찬가지로 화면에 보이지 않는 `<head>` 태그 안의 내용들도 Render Tree에는 추가되지 않는다. 그래서 아래 이미지의 Render Tree에는 `<head>` 태그와, display속성이 none인 `<p>` 태그 하위의 `<span>` 태그가 사라진 것을 확인할 수 있다.

![Render Tree &#xC0DD;&#xC131;](https://post-phinf.pstatic.net/MjAxNzA3MDRfMTUz/MDAxNDk5MDk4ODk0MjIz.SvlNpa9CTDpuHxOVrFlJqminaiJK9zWN0wN9uyXP9vog.voakcaq1COJE6GHchrTqcrQov3QllzhayTRXbB0Su30g.PNG/render-tree-construction.png?type=w1200)

Render Tree에는 사실 여러 가지가 포함되어 있다. Render Object Tree, Render Layer Tree 등등을 합쳐서 화면을 그리는 데에 필요한 모든 정보를 가지고 있는 Render Tree가 완성됩니다. \('등등'에는 Render Style Tree, InlineBox Tree같은 것들도 있다.\)

![https://www.chromium.org/developers/design-documents/gpu-accelerated-compositing-in-chrome ](https://post-phinf.pstatic.net/MjAxNzA3MDRfMjA4/MDAxNDk5MTMyNjA2NzY2.omsYuQxxpcuBTNlQxuqfeaWOPxVow6cvNmSQNSvLwyUg.7-uIJppReNL5w9NIhYkLiOVgmr4KeTKwPty4Znyl7-8g.PNG/the_compositing_forest.png?type=w1200)

Render Object Tree가 위에서 말했듯이 DOM Tree의 노드 중에서 화면에 보이는 것들만으로 이루어지는 트리이다. block, inline, image, text, table같은 요소들이 Render Object가 된다. DOM Tree에서 `<div>`는 Render Object Tree에 Block element로, `<span>`은 Inline element로 옮겨지는 것이다.

Render Object의 속성에 따라 필요한 경우 Render Layer가 만들어진다. 그리고 이 Render Layer중에서 GPU에서 처리되는 부분이 있으면 다시 Graphic Layer로 분리된다. 대표적으로는 다음과 같은 속성들이 쓰였을 때 Graphic Layer가 만들어지게 된다. \('하드웨어 가속'을 사용할 수 있게 되어 성능을 좋게 한다고 할 때가 이 때이다.\)

* CSS 3D Transform\(translate3d, preserve-3d 등\)이나 perspective 속성이 적용된 경우
* `<video>` 또는 `<canvas>` 요소
* CSS3 애니메이션함수나 CSS 필터 함수를 사용하는 경우
* 자식 요소가 레이어로 구성된 경우
* z-index 값이 낮은 형제 요소가 레이어로 구성된 경우

만약 이런거 저런거 하나도 없이 `<div>` 하나에 width정도 속성만 있다고 하면 레이어는 기본으로 만들어지는 하나만 사용하게 된다.

#### 5. 레이아웃

화면에 보이는 노드들만을 가지고 있는 Render Tree가 다 만들어지면, 이제 Render Tree에 있는 각각의 노드들이 화면의 어디에 위치할 지를 계산하는 Layout과정을 거친다. CSSOM에서 가져온 스타일 정보들로 이미 얘가 어떻게 생겨야 한다는 것은 모두 알고 있지만, 그래서 현재 보이는 뷰포트를 기준으로 실제로 놓으려면 얘가 어디에 가야하는 지는 계산을 또 해야하는 것이다. 여기에서 CSS box model이 쓰이며, position\(relative, absolute, fixed..\), width, height 등등 틀과 위치에 관련된 부분들이 계산됩니다. 여기서 메인 스레드는 DOM과 계산된 스타일을 훑어가며 레이아웃 트리를 만든다. 레이아웃 트리는 x, y 좌표, 박스 영역\(bounding box\)의 크기와 같은 정보를 가지고 있다.

만약 width: 50% 로 되어있는데 브라우저를 리사이즈한다고 하면, 보이는 요소들은 변함이 없으니 Render Tree는 그대로인 상태에서, layout단계만 다시 거쳐 위치를 계산해서 그리게 된다.

![&#xB808;&#xC774;&#xC544;&#xC6C3;](https://post-phinf.pstatic.net/MjAxNzA3MDRfMjU1/MDAxNDk5MTQ4MDAyMzcy.lpehOnK3sqhkXDwL8yQ8TnBmZCXvi2CKZFxxMkTeE-gg.71B6TjHwvmGs-g0U44y-qDTKB1ZIMG03ZO0Jky1UG5og.GIF/resize_browser.gif?type=w1200)

> 알아두면 좋은 것 - 레이아웃 트리는 DOM 트리와 비슷한 구조일 수 있지만 웹 페이지에 보이는 요소에 관련된 정보만 가지고 있다. display: none 속성이 적용된 요소는 레이아웃 트리에 포함되지 않는다\(그러나 visibility: hidden 속성이 적용된 요소는 레이아웃 트리에 포함된다\). 이와 비슷하게 p::before{content:"Hi!} 속성과 같은 의사 클래스\(pseudo class\)의 콘텐츠는 DOM에는 포함되지 않지만 레이아웃 트리에는 포함된다.

![https://d2.naver.com/helloworld/5237120](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-05.png)

#### 6. 페인트

DOM, 스타일, 레이아웃을 가지고도 여전히 페이지를 렌더링할 수 없다. 그림을 하나 따라 그리려고 한다고 생각해 보자. 요소의 크기, 모양, 위치를 알더라도 어떤 순서로 그려야 할지 판단해야 한다. 예를 들어 어떤 요소에 z-index 속성이 적용되었다면 HTML에 작성된 순서로 요소를 그리면 잘못 렌더링된 화면이 나온다. 즉, DOM에 선언된 노드 순서와 페인트 순서는 많이 다를 수 있다. 페인트 단계에서 메인 스레드는 페인트 기록\(paint record\)을 생성하기 위해 레이아웃 트리를 순회한다. 페인트 기록은 '배경 먼저, 다음은 텍스트, 그리고 직사각형'과 같이 페인팅 과정을 기록한 것이다. \(퍼포먼스탭에서 볼수있는듯?\)

![&#xD398;&#xC778;&#xD2B8;](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-09.png)

#### 7. 합성

![&#xD569;&#xC131;](https://d2.naver.com/content/images/2019/04/helloworld-201904-sangwoo-ko_3-14.gif)

브라우저는 문서의 구조와 각 요소의 스타일, 요소의 기하학적 속성, 페인트 순서를 알고 있다. 브라우저는 이제 웹 페이지를 어떻게 그릴까? 이 정보를 화면의 픽셀로 변환하는 작업을 래스터화\(rasterizing\)라고 한다.

가장 단순한 래스터화는 아마 뷰포트 안쪽을 래스터하는 것일 것이다. 사용자가 웹 페이지를 스크롤하면 이미 래스터화한 프레임을 움직이고 나머지 빈 부분을 추가로 래스터화한다. 이 방식은 Chrome이 처음 출시되었을 때 래스터화한 방식이다. 그러나 최신 브라우저는 합성\(compositing\)이라는 보다 정교한 과정을 거친다.

합성은 웹 페이지의 각 부분을 레이어로 분리해 별도로 래스터화하고 컴포지터 스레드\(compositor thread\)라고 하는 별도의 스레드에서 웹 페이지로 합성하는 기술이다. 스크롤되었을 때 레이어는 이미 래스터화되어 있으므로 새 프레임을 합성하기만 하면 된다. 애니메이션 역시 레이어를 움직이고 합성하는 방식으로 만들 수 있다. 자세한 정보는 [여기](https://blog.logrocket.com/eliminate-content-repaints-with-the-new-layers-panel-in-chrome-e2c306d4d752) 클릭.\)

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

