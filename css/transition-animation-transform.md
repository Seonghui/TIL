Transition, animation and transform
---

# Transition
* 요소의 변화를 일정 기간(duration)동안 일어나게 함
* 자동으로 발동되지 않음. 특정 이벤트(호버, 클릭 등)에 의해 동작됨

```css
.sqare {
    width: 100px;
    height: 100px;
    background-color: red;

    transition-property: width, background-color; // 트랜지션의 대상이 되는 프로퍼티를 지정 (기본값 all)
    transition-duration: 1.2s, 3s // 변화가 일어나는 기간. 초단위. 프로퍼티와 각각 대응 (기본값 0s)
    transition-timing-function: ease; // 트랜지션 변화율 함수 지정 (기본값 ease)
    transition-delay: 1s; // 트리거 이벤트 발생 후 몇 초 후에 트랜지션이 시작될 것인지 지정 (기본값 0s)
    transition : (shorthand)
}

.sqare:hover {
    width: 300px;
    background-color: blue;
}
```

# Animation
* 이벤트 없이 효과 줄 수 있음
* 축약 가능
```
// animation-name, animation-duration, animation-timing-function, animation-delay, animation-iteration-count, animation-direction  순서
animation: slidein 3s linear 1s infinite alternate;
```

## 예시
1. 키프레임 정의
```css
// from은 0%, to는 100%
@keyframes myAnimation {
    from {
        background-color: red;
        width: 100px;
        height: 100px;
    }
    to {
        background-color: blue;
        width: 200px;
        heigh: 100px;
    }
}

// 이렇게도 정의 가능
@keyframes myAnimation {
    0% {
        background-color: red;
        width: 100px;
        height: 100px;
    }
    30% {
        background-color: yellow;
    }
    100% {
        background-color: blue;
        width: 200px;
        heigh: 200px;
    }
}
```

2. 적용하기
```css
.sqare {
    animation-name: myAnimation; // @keyframes 이름
    animation-duration: 3s // 변화가 일어나는 기간. 초단위. (기본값 0s)
    animation-iteration-count: 3; (기본값 1. number or infinite.)
    animation-timing-function: ease; // 애니메이션 함수 지정 (기본값 ease)

    /*
        애니메이션이 반복될 때 진행 방향을 지정

        normal: from -> to (기본값)
        reverse: to -> from
        alternate: 홀(normal) 짝(reverse)
        alternate-reverse: 홀(reverse) 짝(normal)
    */
    animation-direction: normal;

    /*
        애니메이션이 실행 상태가 아닐 때 (대기 or 종료) 요소의 스타일 지정

        none:       대기 -> 시작 프레임(from)에 설정한 스타일을 적용하지 않고 대기한다.
                    종료 -> 애니메이션 실행 전 상태로 애니메이션 요소의 프로퍼티값을 되돌리고 종료한다.

        forwards:   대기 -> 시작 프레임(from)에 설정한 스타일을 적용하지 않고 대기한다.
                    종료 -> 종료 프레임(to)에 설정한 스타일을 적용하고 종료한다.

        backwrads:  대기 -> 시작 프레임(from)에 설정한 스타일을 적용하고 대기한다.
                    종료 -> 애니메이션 실행 전 상태로 애니메이션 요소의 프로퍼티값을 되돌리고 종료한다.

        both :      대기 -> 시작 프레임(from)에 설정한 스타일을 적용하고 대기한다.
                    종료 -> 종료 프레임(to)에 설정한 스타일을 적용하고 종료한다.
    */
    animation-fill-mode: none;

    // running(기본값) || paused
    animation-play-state: running;
    animation-delay: 1s; // 요소 로딩 후 몇 초 후에 애니메이션이 시작될 것인지 지정 (기본값 0s)
    animation : (shorthand)
}
```

# Transform
* 요소에 이동(translate), 회전(rotate), 획대 축소(scale), 비틀기(skew) 효과를 줄 수 있음

![transform 속성들](https://i.imgur.com/qgaqmoI.png)

## transform
* 여러개 나열할 수 있음. `transform: scale(.5) rotate(20deg);` 이렇게 쉼표없이 나열
* 트랜지션을 주면 움직이게도 할 수 있음

## transform-origin
* 요소의 기본 기준점을 설정할 때 사용. 기본 기준점은 요소의 정중앙 (50%, 50%)

## 3d transform
* 이건 나중에 궁금하면 다시 알아보기


# refs
* https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Animations/Using_CSS_animations
* https://poiemaweb.com/css3-transform
* https://ahribori.com/article/5a0c49926c9eef13d882e3ea
