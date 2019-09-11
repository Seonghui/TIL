---
layout: default
title: Debounce and throttle
parent: 9. 기타
grand_parent: javascript
nav_order: 1
---

# Debounce and throttle

둘은 비슷하지만 다름. 둘다 실행 시간동안 함수를 얼마나 많이 실행시키는지에 대해서 컨트롤함. 예를 들어, 스크롤 이벤트를 실행할 떄 초당 30 이벤트가 넘게 트리거됨. 그런데 스크롤을 아주 느리게 하면 스크롤 이벤트는 초당 100회나 넘게 트리거됨. 이 사단을 해결하는 한가지 방법은 250ms마다 루프를 실행시키고 그 안에 스크롤 이벤트를 넣는 것임. 그러면 스크롤 이벤트는 250ms마다 실행이 되것지요.

아무튼 요새는 더 복잡해져서 Debounce, Throttle, and requestAnimationFrame 등등의 함수를 사용.

## Debounce

여러 개의 순차적 호출을 하나의 그룹으로 그룹화. 예를 들어 엘리베이터를 탈때 문이 닫히고 갑자기 어떤 사람이 갑툭튀로 탐. 그럼 엘리베이터는 올라가지 않을거고, 문은 다시 열림. 그리고 또다른 사람이 갑툭튀로 탐. 그럼 엘리베이터는 올라가는 것을 잠시 멈춤. 사람이 전부 다 타고 조금 지나서야 엘리베이터는 움직이기 시작.

다시 말해 이벤트를 겁나 실행시키면 알아서 하나의 이벤트로 묶어버리고 그 이벤트가 끝나고서야 함수를 실행시키는 것임.

그런데, debouncing 이벤트가 항상 그룹 이벤트가 끝나고 조금뒤에 실행돼서 약간 짱날수도 있음. 하지만 Leading Edge를 사용하면 그룹 이벤트가 끝나자마자 바로 실행되게 만들 수 있음.

### Debounce Example - resizing

브라우저 창을 리사이징할때 많은 resize 이벤트가 실행이 됨. 그런데 아래 예제에서는 0.4ms display info가 변경됨.

```text
// Based on http://www.paulirish.com/2009/throttled-smartresize-jquery-event-handler/
$(document).ready(function(){

  var $win = $(window);
  var $left_panel = $('.left-panel');
  var $right_panel = $('.right-panel');

  function display_info($div) {
    $div.append($win.width() + ' x ' + $win.height() +  '<br>');
  }

  $(window).on('resize', function(){
    display_info($left_panel);
  });

  $(window).on('resize', _.debounce(function() {
    display_info($right_panel);
  }, 400));
});
```

## Throttle

함수가 X ms다 한 번 이상 실행되도록 허용하지 않음. debouncing과 제일 다른 점은, throttle은 함수가 적어도 Xms마다 정기적으로 실행되는걸 보장한다는 것임.

### Throttle Example - Infinite scrolling

유저가 bottom에서 얼마나 먼지를 체크하고 만약 유저가 bottom에 가까워지면 ajax 요청으로 컨텐츠를 더 보여주는 것임. 여기서 디바운싱은 별 쓸모가 없음. 왜냐? 디바운싱을 쓰면 유저가 스크롤을 멈출 때 비로소 트리거되기 때문임. 하지만 여기서의 목표는 bottom에 닿기 전 fetch를 시작하는 것임. 따라서 Throttle를 사용해서 바닥에 얼마나 가까워지는지를 정기적으로 체크해줌.

```text
HTML  CSS  JS Result
EDIT ON
 // Very simple example.
// Probably you would want to use a 
// full-featured plugin like
// https://github.com/infinite-scroll/infinite-scroll/blob/master/jquery.infinitescroll.js
$(document).ready(function(){

  // Check every 200ms the scroll position
  $(document).on('scroll', _.throttle(function(){
    check_if_needs_more_content();
  }, 300));

  function check_if_needs_more_content() {     
    pixelsFromWindowBottomToBottom = 0 + $(document).height() - $(window).scrollTop() -$(window).height();

  // console.log($(document).height());
  // console.log($(window).scrollTop());
  // console.log($(window).height());
  //console.log(pixelsFromWindowBottomToBottom);


    if (pixelsFromWindowBottomToBottom < 200){
      // Here it would go an ajax request
      $('body').append($('.item').clone()); 

    }
  }
});
```

## requestAnimationFrame \(rAF\)

* requestAnimationFrame은 다른 방식의 함수 실행\(rate-limiting the execution\) 방식임. 보다나은 정확성을 위한 브라우저 native API임.throttle의 대안이 될 수 잇음
* Aims for 60fps \(frames of 16 ms\), 그런데 알아서 best timing을 결정함. 심플한 스탠다드 API로 별로 바꿀 필요도 유지보수를 할 필요도 없음.
* rAF를 시작하고 취소하는 건 알아서 해야됨. \(debounce나 throttle는 알아서 해줌\)
* 브라우저 탭이 액티브 상태가 아니면 실행이 안됨
* IE9, 오페라 미니랑 예전 안드로이드 폰에서 실행안됨. 폴리필 필요. node.js에서 지원안됨

### Example

```text
// Based on https://www.html5rocks.com/en/tutorials/speed/animations/#debouncing-scroll-events



var latestKnownScrollY = 0,
    ticking = false,
  item = document.querySelectorAll('.item');


function update() {
    // reset the tick so we can
    // capture the next onScroll
    ticking = false;

  item[0].style.width = latestKnownScrollY + 100 + 'px';
}


function onScroll() {
    latestKnownScrollY = window.scrollY; //No IE8
    requestTick();
}

function requestTick() {
    if(!ticking) {
        requestAnimationFrame(update);
    }
    ticking = true;
}

 window.addEventListener('scroll', onScroll, false);


/// THROTTLE

function throttled_version() {
   item[1].style.width = window.scrollY + 100 + 'px';
}

 window.addEventListener('scroll', _.throttle(throttled_version, 16), false);
```

## 결론

* 요소 위치를 re-calculating 하거나, 프로퍼티에 직접적으로 애니메이팅을 주는 목적에는 requestAnimationFrame이 좋음.
* Ajax 요청이나 클래스 add, remove\(css 애니메이션\)에는 debounce나 throttle를 사용하는 것이 괜츈함.\(where you can set up lower executing rates \(200ms for example, instead of 16ms\)\)

## refs

* [https://css-tricks.com/debouncing-throttling-explained-examples/](https://css-tricks.com/debouncing-throttling-explained-examples/)

