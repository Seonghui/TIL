---
layout: default
title: SVG
parent: etc
nav_order: 2
---

# SVG

## SVG\(Scalable Vector Graphics\)란?

* 2차원 그래픽을 표현하기 위해 만들어진 XML 파일 형식의 마크업 언어
* 코드로 수정 가능
* 최적화하려면 [SVGO](https://github.com/svg/svgo) 사용하자... npm으로 설치 가능
* css로 스타일 조작 가능

## 적용 방식

### img

* 일반 이미지와 같이 취급. 
* 패스 못 봄.
* 조작 기능을 제한.
* 애니메이션 불가능

  ```text
  <img src="bblogo.svg" alt="Breaking Borders Logo" height="65" width="68">
  ```

### background-image

* base64 인코딩을 하면 다운로드하는동안 나머지 스타일 로딩을 차단해서 사용하기 좋지 않다.
* 패스 못 봄
* 조작 기능을 제한
* 애니메이션 불가능

  ```text
  .logo {
  background-image: url(bblogo.svg);
  }
  ```

  ```text
  <!-- base64 인코딩으로 변환한 SVG -->
  <img alt="" src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDo etc">
  ```

### object

인라인 제외 svg를 조작하는 경우에 가장 좋은 방법임

```text
<object type="image/svg+xml" data="bblogo.svg">현재 브라우저는 iframe을 지원하지 않습니다.</object>
```

### inline

HTTP 요청은 저장되지만 캐시되지 않음. 조작 가능.

```text
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 68 65">
  <path fill="#1A374D" d="M42 27v-20c0-3.7-3.3-7-7-7s-7 3.3-7 7v21l12 15-7 15.7c14.5 13.9 35 2.8 35-13.7 0-13.3-13.4-21.8-26-18zm6 25c-3.9 0-7-3.1-7-7s3.1-7 7-7 7 3.1 7 7-3.1 7-7 7z"/>
  <path d="M14 27v-20c0-3.7-3.3-7-7-7s-7 3.3-7 7v41c0 8.2 9.2 17 20 17s20-9.2 20-20c0-13.3-13.4-21.8-26-18zm6 25c-3.9 0-7-3.1-7-7s3.1-7 7-7 7 3.1 7 7-3.1 7-7 7z"/>
</svg>
```

### 기타

iframe, embed는 사용하지 않는 것이 좋음

## refs

* [https://css-tricks.com/using-svg/](https://css-tricks.com/using-svg/)
* [https://svgontheweb.com/ko](https://svgontheweb.com/ko)
* [https://css-tricks.com/lodge/svg/09-svg-data-uris/](https://css-tricks.com/lodge/svg/09-svg-data-uris/)

