# css-specificity

### css 명시도

* 요소를 설명하는 부분은 0,0,0,1
* 클래스, 가상 클래스, 속성을 설명하는 부분은 0,0,1,0
* id를 설명하는 부분은 0,1,0,0
* 인라인 스타일을 나타내는 부분은 0,0,0,0 \(아닐 때는 1,0,0,0\)

### 예시

```text
id [ 100 ] > class [ 10 ] > tag [ 1 ] > * [ 0 ]
```

* div ul ul li: 0,0,0,4
* div.aside ul li: 0,0,1,3
* a:hover: 0,0,1,1
* div.navlinks a:hover: 0,0,2,2
* **title em: 0,1,0,1**
* h1\#title em: 0,1,0,2

쉽게 계산하려면 각 자리수에 점수를 매겨서 곱하면 됨.

```text
h1#title ul li  // 0,1,0,2-> 10
div ul li   // 0,0,0,3 -> 0
```

따라서 `h1#title ul li`이 먼저 선언되었어도 `h1#title ul li`이 적용됨. 만약 명시도가 같을 경우 나중에 적은 규칙이 앞의 규칙을 덮어쓰게 됨. 그런데 명시도와 관계없이 `!important를` 붙이면 우선 순위가 제일 높아짐.

## refs

* 에릭 마이어의 CSS 노하우
* [http://www.nextree.co.kr/p8468/](http://www.nextree.co.kr/p8468/)

