---
layout: default
title: 시멘틱 마크업
parent: html
nav_order: 2
---

# 시멘틱 마크업

### HTML5 Semantic Markup \(의미론적 마크업\)

Semantic HTML이라고도 함. 시맨틱 HTML은 HTML 마크업을 사용하여 의미\(semantics or meaning\)를 강화하는 것임. 이렇게 하면 웹페이지가 단순히 모양을 정의하는게 아니라 의미를 강화할 수 있게됨.

## 여기서 마크업이란

한 덩어리의 텍스트를 태그로 감싸서 추가적인 의미를 부여하는 행위. 그 추가적인 의미에 따라 태그도 달라짐. 즉 중요한 내용이다라는 내용을 추가하고 싶으면 `<strong>`을 쓰고, 이걸 강조하고 싶다는 의미를 추가하려면 `<em>`을 씀.

## header

* `<div id="header">` 보다는 `<header>`를 사용하자
* 페이지 시작부분에 사용함
* 한 사이트 내에 `<header>`를 여러개 가질 수도 있음
* 보통 적어도 하나의 h태그\(`<h1> – <h6>`\)를 포함. 로고나 검색 같은것도 포함할 수 잇음. 요새는 nav 태그를 header 안에 위치시킨다.
* 레거시 브라우저\(ie9보다 아래\)의 경우를 위해 header를 block으로 render되게 하고싶으면 `header { display: block }`이라고 명시적으로 적어줘야 함.

### 예시

```html
 <header>
    <h1>Little Green Guys With Guns</h1>
    <nav>
      <ul>
        <li><a href="/games">Games</a>
        <li><a href="/forum">Forum</a>
        <li><a href="/download">Download</a>
      </ul>
    </nav>
    <h2>Important News</h2> <!-- this starts a second subsection -->
    <!-- this is part of the subsection entitled "Important News" -->
    <p>To play today’s games you will need to update your client.</p>
    <h2>Games</h2> <!-- this starts a third subsection -->
</header>
<p>You have three active games:</p>
```

## footer

* `<div id="footer">` 보다는 `<footer>`
* 사이트마다 여러개의 푸터를 가질수있음

### 예시

```html
<footer>
  <ul>
     <li>copyright</li>
     <li>sitemap</li>
     <li>contact</li>
     <li>to top</li>
  </ul>
</footer>
```

## aside

* aside 요소 주위의 내용과 관련된 내용으로 구성되어 있지만 해당 콘텐츠랑 별개의 내용으로 구성되어있음.
* aside에 들어가기 적절한 내용은 마치 인쇄물에서의 additional notes같은 내용임. 
* aside는 컨텐츠와 연결되어있지만 독립적으로 구성되어야 한다. 그리고 필수적인 요소가 아님.
* 추가적인 정보로 아티클의 내용을 강화하는데 사용하거나, 독자가 관심있어할 정보를 highlighting parts로 나타나는데 사용함. 
* aside가 article 태그 안에 있으면 아티클과 연관된 내용\(어휘 사전 같은\)이 들어가야 하고, 만약 article 바깥쪽에 aside 가 위치하는 경우에는 사이트에 연관된 내용\(추가적인 내비게이션, 광고 등등\)이 들어간다.

### 예시

* pull-quote를 포함하는 aside
* article 내의 aside

  ```html
  <article>
    <header>
        <h1>Lorem Ipsum Dolor Sit Amets</h1>
    </header>
    <p>Aliquam erat volutpat. Vestibulum eleifend pellentesque urna, at 
    sodales est faucibus sit amet. Praesent in mi dui. <q>Aliquam sed 
    bibendum nisl. Mauris pharetra enim sit amet ipsum dictum placerat. Sed 
    lacinia pulvinar iaculis. Nam sit amet hendrerit purus.</q> Sed a urna 
    laoreet lorem pulvinar fermentum. Aenean vel luctus libero. Ut tincidunt 
    metus sagittis ante viverra feugiat.</p>
    <aside>
        <q>Mauris pharetra enim sit amet ipsum dictum placerat.</q>
    </aside>
    <p>Nulla quis lacus non quam luctus vestibulum. Pellentesque imperdiet 
    risus gravida ante consectetur fermentum. Vivamus et est nec risus volutpat 
    elementum. Ut faucibus, lectus consectetur volutpat dapibus, quam diam 
    luctus enim, vitae mollis enim purus non ante.</p>
  </article>
  ```

### 예시

* 컨텐츠와 연관되어있지만 article body에는 넣을 필요 없는 내용들

  ```html
  <article>
    <header>
        <h1>Web Technologies</h1>
    </header>
    <p>Curabitur dignissim lorem a CSS diam posuere tempor. Nam hendrerit, 
    eros vel condimentum tempor, ipsum justo cursus justo, quis vestibulum 
    turpis turpis sit amet tellus. Quisque quis PHP magna eget ipsum faucibus 
    bibendum at non diam. Sed sapien est, cursus ac ullamcorper id, egestas vel 
    urna JavaScript. Nullam aliquam dolor vitae quam pharetra auctor.</p>
    <aside>
        <dl>
            <dt>CSS</dt>
            <dd>A set of standards for styling documents presented on the 
            World Wide Web.</dd>
            <dt>PHP</dt>
            <dd>A server-side scripting language suited to dynamic HTML document 
            generation for the web.</dd>
            <dt>JavaScript</dt>
            <dd>A client-side scripting language used for manipulating HTML documents 
            within a browser.</dd>
        </dl>
    </aside>
  </article>
  ```

### 예시

* blogroll로 사용된 aside
* article 바깥쪽에 위치

  ```html
  <body>
  <header>
    <h1>My Blog</h1>
  </header>
  <article>
    <h1>My Blog Post</h1>
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do
    eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
    <aside>
      <!-- Since this aside is contained within an article, a parser
      should understand that the content of this aside is directly related
      to the article itself. -->
      <h1>Glossary</h1>
      <dl>
        <dt>Lorem</dt>
        <dd>ipsum dolor sit amet</dd>
      </dl>
    </aside>
  </article>
  <aside>
    <!-- This aside is outside of the article. Its content is related
    to the page, but not as closely related to the above article -->
    <h2>Blogroll</h2>
    <ul>
      <li><a href="#">My Friend</a></li>
      <li><a href="#">My Other Friend</a></li>
      <li><a href="#">My Best Friend</a></li>
    </ul>
  </aside>
  </body>
  ```

## nav

* 말그대로 내비게이션 링크에 사용. 그런데 모든 링크를 적을 필요는 없고, 주요 링크만 적으면 됨. \(primary navigation\)
* nav 태그 여러 개 써도 됨

### 예시

```html
<nav>
  <ul>
    <li><a href="index.html">Home</a></li>
    <li><a href="/about/">About</a></li>
    <li><a href="/blog/">Blog</a></li>
  </ul>
</nav>
```

## section

* 컨텐츠가 문서의 아웃라인에 명시적으로 리스트될때만 section 태그 사용. 서로 관계있는 문서를 분리하는 역할을 함.
* section 요소에는 h태그\(h1–h6\)를 자식으로 포함해야 한다.
* 스타일링이나 스크립팅하기위해서 사용하지 말것. 이렇게 사용하려면 div가 적당
* 제목이 없으면 사용하지 말것
* 블로그 포스트나 코멘트, 뉴스 기사 같은건 article을 사용하기

## The i, b, em, strong

`<i>`와 `<b>`, `<em>` `<strong>`은 뭔 차이일까? 참고로 `<i>`와 `<b>`는 html4 요소들이고 html5에서 재정의되었음. 봐도 헷갈리는데 직접 사용하려면 html5 스펙 봐야할듯;;

* `<i>`: 이탤릭\(italic\). 그런데 html5에서는 이탤릭체를 의미하지 않음. 그냥 주변의 문장과 다른 의미나 표현을 나타내기 위해 사용. 시각적 또는 청각적 구분이 필요한 곳에 사용.
* `<b>`: 볼드\(boid\). i 태그와 같이 html5에서는 그저 다른 문장과 구별되는 문체를 가지는 텍스트에 이용됨.
* `<em>`: 강조\(emphasis\). 그냥 어떠한 문구에 강조와 이탤릭문구를 사용하고 싶을 때. 고양이는 귀여운 동물입니다에서 귀여운을 감싸기 좋음. 문맥을 통해 의미 변화를 이끌어내기 위한 요소.
* `<strong>`: 강한 강조\(stronger emphasis\). 키워드같은 것을 강조하기 좋음. em은 문맥을 바꾸지만 strong은 문백을 바꾸지 않음.

## hr

* end of one section, start of another, which is the same semantically as `</section><section>`. 
* `<hr>` is more for thematic breaks
* 다른 토픽의 섹션이 나올때 사용하기 좋음
* hr을 css 사용해서 이미지로 바꿔 사용할 수도 있음

## article

* 문서나 페이지의 구성 요소임
* 포럼 포스트, 매거진이나 뉴스 아티클, 블로그 시작페이지, 코멘트, 위젯 등등 콘텐츠 독립적인 콘텐츠 항목에 대해서 사용할 수 잇음.

### 사용 방식 예시

너무 많아서 [예시](http://html5doctor.com/the-article-element/) 보는게 나을듯. 리스트는 대략...

* h태그와 p태그를 포함한 article
* 웹로그 스타일
* nested article을 포함한 코멘트 article
* section을 포함한 article
* section 안에 있는 article
* 위젯으로 사용한 article

### article과 section의 차이

* article은 전문화된 section 같은 거다. section보다는 더 분명한 semantic meaning이 있다. 콘텐츠가 독자적인 내용을 가지고 있디면 article을 사용하고, 콘텐츠와 관련이 있는 내용이라면 section을 사용하면 된다. 만약 의미적으로 아무 연관도 없는 블록 형태라면 div를 사용하면 된다.

## dl

* html5에서는 definition list로 정의되었지만 html5에서 description list로 재정의됨. 
* names and values의 many-to-many relationship을 표현. 
* `<dl>`은 많은 키 \(`<dt>`\)를 여러 값 \(`<dd>`\)으로 매핑 가능

```html
<dl>
  <dt>Authors:</dt>
  <dd>Remy Sharp</dd>
  <dd>Rich Clark</dd>
  <dt>Editor:</dt>
  <dd>Brandan Lennox</dd>
  <dt>Category:</dt>
  <dd>Comment</dd>
</dl>
```

## div

* 시맨틱하지 않은 부분에 사용\(예를 들면 site wrapper나 스타일링을 주고 싶을 때.\)

## main

HTML `<main>` 요소는 문서나 앱 `<body>`의 주요 콘텐츠를 나타냄. 주요 콘텐츠 구역은 문서의 핵심 주제나 애플리케이션의 핵심 기능성에 대해 부연, 또는 직접적으로 연관된 콘텐츠들로 이루어짐.

## 예시

```text
<body>
<header role="banner">
[...]
</header>

<div id="content" class="group" role="main">
[...]
</div>

<footer role="contentinfo">
    [...]
</footer>
</body>
And here's what it's like now:

We'll get around to removing the id="content" at some point too.
<body>
<header role="banner">
[...]
</header>

<main id="content" class="group" role="main">
[...]
</main>

<footer role="contentinfo">
    [...]
</footer>
</body>
```

## refs

* [https://en.wikipedia.org/wiki/Semantic\_HTML](https://en.wikipedia.org/wiki/Semantic_HTML)
* [http://html5doctor.com/](http://html5doctor.com/)

