---
layout: default
title: 메타 태그 (Meta tag)
parent: html
nav_order: 2
---

# 메타 태그 (Meta tag)

* HTML 문서의 정보를 제공
* 페이지에 표시는 안 되지만, machine parsable
* 주로 페이지 설명, 키워드, 페이지 작성자, 마지막 수정일 등등등이 작성됨
* 브라우저, 검색 엔진 등등 이 데이터를 사용한다
* 항상 `<head>` 태그 안에 위치해있어야 한다
* name/value 쌍으로 작성이 되어야 한다

```html
<!DOCTYPE html>
    <html>
    <head>
      <meta charset="UTF-8">
      <meta name="description" content="Free Web tutorials">
      <meta name="keywords" content="HTML,CSS,XML,JavaScript">
      <meta name="author" content="John Doe">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
    </head>
    <body>
      <p>All meta information goes in the head section...</p>
    </body>
    </html>
```

## viewport

* The viewport is the user's visible area of a web page.
* 기기마다 다르고, 폰에서는 컴퓨터 화면보다 작게 보인다
* 뷰포트는 메타 태그 안에 있어야 된다.
* `width=device-width`는 기기의 스크린 width 값을 따라간다는 뜻이다. (기기의 width값마다 달라진다는 말)
* `initial-scale=1.0`는 웹페이지가 로드될 때의 줌레벨을 설정하는 것이다.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

## 렌더링 모드(X-UA-Compatible)

아래 태그로 어떤 렌더링 모드로 작동하게 될지를 강제함. 항상 doctype 과 함께 사용

```html
 <meta http-equiv='X-UA-Compatible" content="IE=edge'>
```

content 값에 지정할 수 있는 값은 아래와 같다.

* **IE=5** : 관용모드(quirks mode)로 지정된 DOCTYPE에 상관없이 IE5 렌더링 방식이 사용된다.
* **IE=7** : IE7 표준모드로 지정된 DOCTYPE에 상관없이 IE7 표준 모드 렌더링 방식이 사용된다.
* **IE=EmulateIE7** : IE7 에뮬레이션 모드로 지정된 DOCTYPE에 따라 IE7 표준모드나 관용모드로 렌더링된다.
* **IE=8** : IE8 표준모드로 지정된 DOCTYPE에 상관없이 IE8 표준모드로 렌더링된다.
* **IE=EmulateIE8** : IE8 에뮬레이션 모드로 지정된 DOCTYPE에 따라 IE8 표준모드나 관용모드로 렌더링된다.
* **IE=edge** : 최신모드로 지정된 DOCTYPE에 상관없이 IE8 이상 버전에서 항상 최신 표준 모드로 렌더링된다.

여기서 주로 거의 사용되는 것은 `IE=edge`

edge라고 지정하면 IE브라우저의 최신 버전 엔진을 사용하라는 말이다. 만약 구형 브라우저를 위한 개발을 하려면 특정 모드를 지정하면 된다. 지정하면 그 버전으로 해당 웹을 호환성 보기로 오픈한다.

## Open Graph(OG)

웹사이트의 미리보기와도 같다. 매우 잘 설명된 글이 있으니 [링크](http://blog.airbridge.io/open-graph-as-a-website-preview/) 참고하기.

## References

* [https://www.w3schools.com/tags/tag_meta.asp](https://www.w3schools.com/tags/tag_meta.asp)
* [https://webdir.tistory.com/38](https://webdir.tistory.com/38)
