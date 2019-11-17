---
layout: default
title: script의 async와 defer
parent: 9. 기타
grand_parent: javascript
nav_order: 50
---

# script의 async와 defer

웹 브라우저는 html을 렌더링하는 과정에서 스크립트나 스타일시트를 만나면 동기적으로 처리함. 다시 말해 해당 내용이 해석되고 실행되기 전에는 뒤에 나오는 내용을 처리하지 않음. 이건 화면 렌더링 속도에 영향을 크게 미침.

## 일반적인 실행

 

![common](https://blog.asamaru.net/res/img/post/2017/05/script-async-defer-1.png)

스크립트를 가져오느라 HTMl 구문 분석이 일시 중단

## async를 추가한 상황

 

![async](https://blog.asamaru.net/res/img/post/2017/05/script-async-defer-2.png)

스크립트 파일이 비동기적으로 실행될 수 있음을 나타내기 위해 사용. HTML 구문 분석과 병행하여 스크립트를 가져온 후 스크립트가 준비될 때마다 즉시 실행. 실행속도가 다운로드 완료 시점에 결정되니까 실행 순서가 중요한 스크립트들을 사용할 때는 주의해서 사용해야 함.

```text
<script async src="script.js">
```

## defer 속성을 추가한 상황

 

![defer](https://blog.asamaru.net/res/img/post/2017/05/script-async-defer-3.png)

HTML 구문 분석이 실행되는 동안 파일을 다운로드. 그리고 구문 분석이 완료되기 전에 스크립트가 다운로드 완료되더라도 구문 분석이 완료될때까지 스크립트는 실행되지 않음. 그리고 호출 순서대로 실행.

```text
<script defer src="script.js">
```

## 언제 사용?

body 태그가 닫히기 직전 스크립트를 넣는 거라면 async나 defer가 크게 의미없음. 다른 파일들에 종속적이지 않거나 종속성 자체가 없는 스크립트에 경우 async 속성이 유용. 어쨌든 스크립트를 헤드쪽에 위치시켜야 되는 경우에 유용하게 쓸 수 있음.

## 브라우저 호환

IE 10 이하 제외 웬만한 브라우저는 전부 사용 가능

