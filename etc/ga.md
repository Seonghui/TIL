Google Analytics
---

# 생김새
```js
<!-- Google Analytics -->
<script>
(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
})(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

ga('create', 'UA-XXXXX-Y', 'auto');
ga('send', 'pageview');
</script>
<!-- End Google Analytics -->
```

1. analytics.js 파일을 다운로드
2. ga 함수를 초기화해서 구글 애널리틱스 기능을 사용할수있게 준비
3. ga('create', 'UA-XXXXX-Y', 'auto')로 트래커 오브젝트를 생성해서 대기열에 추가. UA-XXXXX-Y는 고객의 아이디(트래킹 아이디)같은 것.
4. ga('send', 'pageview')는 현재 페이지에 대한 pageview를 구글 애널리틱스에 보냄

## create 메소드로 할 수 있는 것들
* 트래커 이름 정해주기 (여러개의 트래커를 돌릴때 좋음. 고객용트래커, 마케팅용트래커.. 등등등)
```
// 등록
ga('create', 'UA-XXXXX-Y', 'auto');
ga('create', 'UA-XXXXX-Z', 'auto', 'clientTracker');

// 페이지뷰
ga('send', 'pageview');
ga('clientTracker.send', 'pageview');
```
* create 메소드 작성시에 옵셔널한 필드 오브젝트 작성하기

# count 기준 (세션 단위)
* 사용자의 단위는 세션 단위로 확인됨
* 30분간 연속적인 활동이 있으면 단 하나의 세션으로 취급, 외에는 분리
* 다른 경로로 다시 들어오게 되면 분리 (네이버 광고 클릭해 들어왔다 30분 안에 다음 광고 타고 클릭해 들어오면 2개의 세션으로 처리)
* 날짜가 변경되면 분리 (첫 번째 세션은 8월 14일 오후 11시 59분 59초에 완료되고, 두 번째 세션이 8월 15일 오전 12시 정각에 시작)

# UTM (Urchin Traffic Monitor)
* 주소 자체가 출처를 담는 체계
* 링크를 어디서 타고 들어왔는지 전부 나열하면 길고 정신없음
* 따라서 구글이 미리 정해놓은 규칙에 따라 분류
* 그런데 문제가 있음. 네이버에 등록한 광고는 그냥 자연 검색으로 잡힘. 왜냐면 구글은 미국을 기준으로 키워드 광고를 정했기 때문
* 따라서 utm을 붙여 자동분류가 되게 함
* utm 생성기도 있음 ([클릭](https://support.google.com/analytics/answer/1033867))


## 예시
```
https://www.pokemon.com?utm_source=pikachu&utm_medium=raichu&utm_campaign=charmander&utm_content=squirtle
```
GA에는 아래와 같이 기록됨. source와 medium이 제일 중요

* utm_source: pikachu
* utm_medium: raichu
* utm_campaign: charmander
* utm_content: squirtle

| - | - | - | - |
|----------|--------------------------------|-----------------|----------------------------|
| source   | 이 트래픽이 어디로부터 왔나    | naver daum      | 경로         |
| medium   | 이 트래픽이 어떤 방법으로 왔나 | email, ppc      | 어떤 타입의 광고인지       |
| campaign | 특별히 진행하는 캠페인인가     | spring_5per_off | 기간 한정 실험할 때 (세일) |
| term     | 어떤 단어로 검색해서 왔나 | 할인 | 검색어 | 
| content  | 어떤 내용을 보고 왔나 | ad_type_a | a/b 성과 측정할 때


# 일반 유저 인터렉션 트래킹하기
## 현재 페이지의 경로를 포함해 보내기
```
ga('send', 'pageview', location.pathname);
```

## url 수정하기
만약 이렇게 중간에 변하는 값이 들어가는 경우
```
/user/profile
/user/account
/user/notifications
```

이렇게 URL을 수정 가능함
```
if (document.location.pathname.indexOf('user/' + userID) > -1) {
  var page = document.location.pathname.replace('user/' + userID, 'user');
  ga('send', 'pageview', page);
}
```

그럼 이렇게 보임
```
/user/profile
/user/account
/user/notifications
```


# refs
- https://www.slideshare.net/yongho/ga-47277482
- https://developers.google.com/analytics/devguides/collection/analyticsjs/
- https://support.google.com/analytics/answer/2731565?hl=ko