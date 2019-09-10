---
layout: default
title: SASS
parent: css
nav_order: 3
---

## 중첩된 속성

padding- margin 같은 같은 네임스페이스를 사용하는 속성은 아래와 같이 사용 가능

```text
.box {
  font: {
    weight: bold;
    size: 10px;
    family: sans-serif;
  };
  margin: {
    top: 10px; // margin-top: 10px
    left: 20px;
  };
  padding: {
    bottom: 40px;
    right: 30px;
  };
}
```

## CSS calc\(\)

상대적 단위의 연산의 경우 CSS calc\(\) 사용

```text
width: 50% - 20px;  // 단위 모순 에러(Incompatible units error)
width: calc(50% - 20px);  // 연산 가능
```

## 나누기 연산 주의하기

/ 나누기 기호를 연산으로 사용하려면, 값이 변수에 저장되거나 함수에 반환되거나, 값이 \(\)로 묶여있거나, 값이 다른 산술 표현식의 일부로 사용되어야 함

```text
div {
  $x: 100px;
  width: $x / 2;  // 변수에 저장된 값을 나누기
  height: (100px / 2);  // 괄호로 묶어서 나누기
  font-size: 10px + 12px / 3;  // 더하기 연산과 같이 사용
}
```

## 가변 인수

mixin 같은거 만들때 인수의 개수가 불확실할 때가 잇음. 그럴때 가변 인수를 사용하면 된다.

```text
// 인수를 순서대로 하나씩 전달 받다가, 3번째 매개변수($bg-values)는 인수의 개수에 상관없이 받음
@mixin bg($width, $height, $bg-values...) {
  width: $width;
  height: $height;
  background: $bg-values;
}

div {
  // 위의 Mixin(bg) 설정에 맞게 인수를 순서대로 전달하다가 3번째 이후부터는 개수에 상관없이 전달
  @include bg(
    100px,
    200px,
    url("/images/a.png") no-repeat 10px 20px,
    url("/images/b.png") no-repeat,
    url("/images/c.png")
  );
}
```

## content

선언된 Mixin에 @content이 포함되어 있다면 해당 부분에 원하는 스타일 블록을 전달할 수 있음

```text
@mixin icon($url) {
  &::after {
    content: $url;
    @content;
  }
}
.icon1 {
  // icon Mixin의 기존 기능만 사용
  @include icon("/images/icon.png");
}
.icon2 {
  // icon Mixin에 스타일 블록을 추가하여 사용
  @include icon("/images/icon.png") {
    position: absolute;
  };
}
```

## extend

되도록이면 사용하지 말자. 득보다 실이 더 많다. [가이드](https://sass-guidelin.es/ko/#extend)에서도 사용하지 말라고 함. extend를 사용하면 원치 않은 부작용이 생길 수도 있고, 이 확장으로 많은 css가 생성될수도 있음. 그러니까 이걸 쓰는 대신에 mixin을 사용해보자. 정 쓰고싶다면 extend가 무엇인지 잘 알아보고 적절하게 써야함.

## mixin과 함수의 차이

mixin은 스타일을 반환하고 함수는 연산된 특정 값을 반환한다.

```text
// Mixins
@mixin 믹스인이름($매개변수) {
  스타일;
}

// Functions
@function 함수이름($매개변수) {
  @return 값
}

// Mixin
@include 믹스인이름(인수);

// Functions
함수이름(인수)

// Function Example
$max-width: 980px;

@function columns($number: 1, $columns: 12) {
  @return $max-width * ($number / $columns)
}

.box_group {
  width: $max-width;

  .box1 {
    width: columns();  // 1
  }
  .box2 {
    width: columns(8);
  }
  .box3 {
    width: columns(3);
  }
}
```

## for from through와 for from to의 차이

```text
// through
// 종료 만큼 반복
@for $변수 from 시작 through 종료 {
  // 반복 내용
}

// to
// 종료 직전까지 반복
@for $변수 from 시작 to 종료 {
  // 반복 내용
}

// 1부터 3번 반복
@for $i from 1 through 3 {
  .through:nth-child(#{$i}) {
    width : 20px * $i
  }
}

// 1부터 3 직전까지만 반복(2번 반복)
@for $i from 1 to 3 {
  .to:nth-child(#{$i}) {
    width : 20px * $i
  }
}
```

## each

List와 Map 데이터를 반복할 때 사용. for~in하고 유사

```text
@each $변수 in 데이터 {
  // 반복 내용
}

$fruits: (apple, orange, banana, mango);

.fruits {
  @each $fruit in $fruits {
    li.#{$fruit} {
      background: url("/images/#{$fruit}.png");
    }
  }
}
```

## sass 내장함수

[클릭](https://sass-lang.com/documentation/Sass/Script/Functions.html)

## refs

* [https://sass-guidelin.es/ko](https://sass-guidelin.es/ko)
* [https://heropy.blog/2018/01/31/sass/](https://heropy.blog/2018/01/31/sass/)

