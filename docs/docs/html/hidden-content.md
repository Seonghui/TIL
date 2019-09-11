---
layout: default
title: Hidden Content
parent: html
nav_order: 2
---

# Hidden Content

## 왠지 컨텐츠를 숨겨야할것같은 4가지 상황들

1. 스크린리더를 사용하든말든 모든사람에게 내용을 숨기고 싶은 경우
2. 스크린 리더를 사용하는 사람에게만 콘텐츠 내용을 보여주지 않을 경우
3. 스크린 리더를 사용하는 사람들에게 추가적인 콘텐츠 정보를 주고싶은 경우
4. 특정 스크린 사이즈에서 콘텐츠를 숨기는 경우

## 1. 모든 사람들에게 콘텐츠 숨기기

* html5 hidden 속성 사용
* Browsers won't render elements with the hidden attribute set.
* IE11 이하 지원 안함
* IE10 이하 지원하려면 hidden에 `display: none; !important` 속성 줘야함

```text
<div class="example" hidden></div>

<style>
  [hidden] {
    display: none !important;
  }
</style>
```

## 2. 스크린 리더 사용자에게 콘텐츠 숨기기

* 보통 디자인적인 요소\(아이콘\)를 숨김
* `aria-hidden` 사용. true일때 숨겨짐

```text
<div class="my-glyph" aria-hidden="true"></div>
```

## 3. 스크린 리더 사용자에게 추가 정보 보여주기

* visual clue로 정보를 주는 경우에는 스크린리더 유저가 읽을수가 없음. 따라서 스크린 리더 유저들에게 비슷한 정보를 전달해줘야 함.
* 예를 들어, 페이지네이션은 스크린 리더 유저들에게 의미없는 정보처럼 느껴질수도 있음. 따라서 스크린 리더 유저들에게 추가적인 정보를 주는것이 도움이 될수있음.
* `display: none`을 속성을 주면 스크린 리더 유저들도 읽을 수 없음. 따라서 아래와 같이 하면 됨

  ```text
  .sr-only {
  border: 0; 
  clip: rect(0 0 0 0); 
  clip-path: polygon(0px 0px, 0px 0px, 0px 0px);
  -webkit-clip-path: polygon(0px 0px, 0px 0px, 0px 0px);
  height: 1px; 
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  width: 1px;
  white-space: nowrap;
  }
  ```

## 4. 특정 스크린 사이즈에서 숨기기

특정 스크린에서 숨기는 경우에는 콘텐츠가 접근성 트리에 전부 들어있어야하니까 hidden이나 aria-hidden이 불필요.

## refs

[https://cloudfour.com/thinks/see-no-evil-hidden-content-and-accessibility/\#fn-5450-3](https://cloudfour.com/thinks/see-no-evil-hidden-content-and-accessibility/#fn-5450-3)

