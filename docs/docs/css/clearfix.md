---
layout: default
title: 가상 선택자와 클리어픽스
parent: css
nav_order: 3
---

## 가상 선택자와 가상 요소

요소에 직접적으로 클래스를 부여하지는 않지만 상태에 따라서 클래스를 적용한 것처럼 만들 수 있음. `a:hover` 여기서 `:hover` 이건 가상 선택자임. 그런데 `.icon::after` 여기서 `::after`이건 가상 요소임. 가상 요소를 쓸 때 :: 이렇게 쓰는 경우도 있고 : 이렇게 쓰는 경우도 있는데 원칙은 ::라고 함

### before

해당 요소 시작 전에 content 속성 집어넣기

### after

해당 요소 끝난 다음에 content 속성 집어넣기

### before와 after의 공통 속성

`content=""`를 무조건 가지고 있어야 됨. 콘텐츠 내용에는 문자열 이미지 등등등 다양하게 포함할 수 있음. 말로는 어려우니 [예시](https://www.w3schools.com/cssref/tryit.asp?filename=trycss_content)를 보자.

## clearfix

가상 선택자를 이용한 clearfix 방법임

### 기본 개념

```css
.clearfix::after {
  content: "";
  clear: both;
  display: table;
}
```

float을 적용한 요소의 부모 요소에 빈 컨텐츠를 넣고 float를 초기화\(`clear: both`\)를 해줌. 이때 내용이 빈 콘텐츠에 block 속성을 주어야 함. 예시에서는 `display:table`속성을 주었는데 `display: block` 속성을 주어도 됨. 아무튼 다시 말하자면 부모 요소에 clearfix 속성을 적용해 부모 요소 다음에 빈 block 속성의 컨텐츠를 집어넣는 것임.

![clearfix](https://i.imgur.com/aOAWSIy.png)

### Micro clearfix hack

> 유명한 clearfix 핵인 [Micro clearfix hack](http://nicolasgallagher.com/micro-clearfix-hack/demo/)

앞의 예제와 다른 건, before 속성에다가도 content를 추가했다는 거임. 그리고 IE 6/7을 위한 zoom 속성이 추가되엇다는 것 정도...

> zoom 속성은 가상요소를 지원하지 않는 IE 6/7을 위한 속성임. non-standard 속성이고 IE7까지만 지원. zoom 속성을 가진 요소는 IE 브라우저에서만 존재하는 hasLayout 속성을 가지게 해줌. 옛날 IE는 CSS가 등장하면서 렌더링 엔진이 작살나버렸고, 이 작살난 엔진으로 element를 인식할 수 있게 해주는 속성이 바로 hasLayout임. 따라서 zoom 속성을 줘서 hasLayout 속성을 가지고 element로 인식하게 만들고 float 속성을 포함하게 만들어줌.

그런데 설명에서 :before 는 노쓸모라고 함. 단지 :before을 추가한 이유는 모던 브러우저에서 레이아웃이 망가질 때 생기는 top-margins를 방지하기 위해서라는데 무슨 소리인지 모르겟음. \(탑마진이 생긴다고?? 왜??\) 아무튼 :before을 추가했을때의 장점이 두 가지가 있는데...

1. 시각적 일관성 유지
2. IE 6/7에서의 시각적 일관성 유지\(zoom:1 속성이 적용될 때\)

### flexbox로 비슷하게 하는 방법

```css
.parent {
  display: flex;
  justify-content: center;
}
```

이렇게 하면 굳이 float 안 쓰고 요소 두 개 양사이드로 정렬할 수 있음. 단, ie가 지원되지 않을 수 있으니 확인하고 쓰자.

## References

* [http://nicolasgallagher.com/micro-clearfix-hack/](http://nicolasgallagher.com/micro-clearfix-hack/)
* [http://takeuu.tistory.com/60](http://takeuu.tistory.com/60)
* [https://stackoverflow.com/questions/1794350/what-is-haslayout](https://stackoverflow.com/questions/1794350/what-is-haslayout)
* [https://www.webmasterworld.com/html/3934282.htm](https://www.webmasterworld.com/html/3934282.htm)
* [http://blog.wystan.net/2007/08/14/understanding-haslayout-property-and-holly-hack](http://blog.wystan.net/2007/08/14/understanding-haslayout-property-and-holly-hack)
 -->
