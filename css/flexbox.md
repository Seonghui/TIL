# Flexbox
* flex box에서 중요한 건 축이다. 축을 기준으로 element들을 정렬하는 것이다. 축은 `flex-direction` 속성을 사용해서 지정할 수 있다.

# 속성

## 축 및 순서 관련
### flex-direction
진행 축과 교차 축을 결정한다.

### flex-wrap
싱글라인인지 멀티라인인지를 결정함. 만약 싱글라인일 경우 element가 한 줄에 다 들어가도록 자동적으로 크기 조절이 됨

### flex-flow
`flex-direction`과 `flex-wrap`을 합친 속성임. 순서는 direction 먼저, 그 다음이 wrap.

### order
특정 element의 순서 변경


## 정렬 관련
### justify-content
진행 축의 정렬과 간격을 제어

### align-items
교차 축의 정렬을 제어

### align-self
특정 element의 정렬을 제어

### align-content
여러 줄의 정렬과 간격을 제어

# References
* https://joshuajangblog.wordpress.com/2016/09/19/learn-css-flexbox-in-3mins/
* https://flexboxfroggy.com/#ko
* https://yoksel.github.io/flex-cheatsheet/
* https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Flexible_Box_Layout/Flexbox%EC%9D%98_%EA%B8%B0%EB%B3%B8_%EA%B0%9C%EB%85%90
* https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Flexible_Box_Layout/Flexbox%EC%9D%98_%EA%B8%B0%EB%B3%B8_%EA%B0%9C%EB%85%90