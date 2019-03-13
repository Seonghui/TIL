모달창 활성화되었을 때 body 스크롤 막기
---

많이 사용하는 방법은 body나 html에 `overflow:hidden` 속성을 주는 것이지만, 이렇게 할 경우 iOS 대응이 되지 않음.
따라서 모달이 활성화되었을 때, html과 body를 fix 시키고 현재 창의 스크롤 높이만큼 top 값을 주면 됨.
여기서 top 값을 주지 않을 경우 모달이 활성화되었을 때 스크롤이 알아서 최상단으로 이동하기 때문에 별로 좋지 않음.

# jQuery 예시

* scss
    - locked 클래스에 아래 속성을 주고, 모달이 활성화되었을 때 html에 locked 클래스를 추가하기.
```
	&.locked {
		position: fixed;
		width: 100%;
		overflow-x: hidden;
		overflow-y: auto;
	}
```

* jquery
    - 현재 스크롤 위치를 구해 top 위치 설정
```
var scroll_pos = $(window).scrollTop();
$('html').addClass('locked').css('top', - scroll_pos + 'px');
```

# refs
https://stackoverflow.com/questions/47919557/prevent-body-scrolling-on-mobile-device