---
layout: default
title: IR (Image Replacement)
parent: html
nav_order: 2
---

# IR (Image Replacement)

이미지에 대체 텍스트를 제공하는 것.

* html 요소를 사용하는 경우에는 alt 속성으로 제공 가능

## Visibility: hidden과 display: none은 올바른 IR기법이 아니다

* 이 두 요소는 화면에서 적용된 요소들을 보이지 않도록 하는 공통점을 가지고 있음.
* 하지만 이 두 속성은 스크린 리더에서도 읽지 않는 것을 윈칙으로 함. 왜냐면 사용자의 조작에 따라 특정 html 콘텐츠를 화면에 보이게 하거나 숨기는 기능을 js로 구현하는데, 만약 스크린리더가 이 두 속성이 적용된 요소를 무시하고 출력하면 뭐가 현재 화면에 표시되는 콘텐츠인지 구별할 수 없게 됨

## 올바른 IR기법은 무엇이 있을까

### text-indent

`span{text-indent: -9999’}`

* text-indent 속성 값에 현재 사용되고 있는 디스플레이의 해상도보다 작은 음의 정수 값을 줌
* 사용하기 간편하지만 사용자의 단말기에 따라 이미지가 제대로 로드되지 않을 때 스크린리더 사용자가 아니라 하더라도 이미지를 설명하는 텍스트를 보고 콘텐츠의 내용을 확인해야 하는데 그렇게 할 수 없음
* text-indent 속성이 적용된 요소가 많아지만 웹페이지 로드시 그만큼 위치값을 많이 계산해야돼서 성능 저하가 일어남

### overflow:hidden

* 원리상 text-indent와 유사.

### 높이와 넓이를 0으로 설정

### z-index

* z-index 속성 값을 음의 정수로 설정
* 특정 브라우저에서 css를 끄거나 제대로 로드되지 않을 경우 숨겨진 텍스트가 화면에 노출.

## refs

[https://nuli.navercorp.com/sharing/blog/post/1132804](https://nuli.navercorp.com/sharing/blog/post/1132804)
