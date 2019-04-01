검색엔진최적화 (SEO)
---

# 개념
* 검색엔진이 자료를 수집하고 순위를 매기는 방식에 맞게 웹페이지를 구성해서 검색 결과의 상위에 나올 수 있도록 하는 작업
* 특정한 검색어를 웹페이지에 적절하게 배치하고, 다른 웹페이지에서 링크가 많이 연결되도록 하는 것
* 네이버 같은 국내 사이트의 경우, 검색 엔진의 우선순위 배치에 해외와 다른 기준을 적용하고 있어서 보편적인 SEO 적용시 스팸으로 분류될수도 있음. 왜냐면 자사 서비스에 노출 우선순위를 두었기 때문임.

# title 태그
* `<head>` 태그 사이에 위치
* 타이틀 태그의 내용은 검색 결과에 반영
* 각각의 페이지마다 고유한 제목을 만드는 것이 이상적
* title 태그의 콘텐츠는 주로 결과의 첫번째 행에 나타남

# description 메타 태그
* 내용 미리보기로 사용
* 페이지 제목과 URL 아래쪽에 나타나고, 검색어가 미리보기에 포함되어있으면 굵게 표시됨
* 각 페이지마자 내용에 맞는 고유한 설명을 사용할 것 (동일한 내용은 XX)

![검색 결과의 title과 description 사진](https://i.imgur.com/enXKj5s.png)

# og 태그
* 오픈그래프 태그(Open Graph)는 
* sns에서 웹페이지 URL이 공유될 때 웹페이지의 주요 정보(제목, 이미지, 설명)가 표기되는 방식을 관리
* sns 마다 설정방식이 다를 수도 있음
* 페북에서 [디버거](https://developers.facebook.com/tools/debug/sharing/) 제공

# URL 구조 개선
* URL 아무렇게나 만들지 말자 (ex. folder/blah/hello)
* URL을 잚 만들면 문서를 크롤링하기도 쉬워진다

## URL 구조를 위한 권장사항
* URL에 단어 사용 (긴 url, page1, this-is-long-url 같은 건 쓰지말자..)
* 단순 디렉토리 구조 만들기 (의미없는 중첩 피하기)
* 특정 문서에는 한 가지 형태의 URL만 있게 하자.. 서로 다른 버전의 URL을 가지고 페이지에 링크하는 경우 해당 내용에 대한 인지도가 여러 URL로 분산될 수가 있기 때문에 하나의 페이지에는 하나의 URL만을 사용해서 참조하는 것이 좋음.

## 사용자가 url의 일부를 제거하는 경우
* 적절하게 상위 링크로 이동시켜주거나 홈으로 이동시켜주거나 하자.. 만약 별 방법이 없을 경우 404페이지를 보여줌

## 페이지 이동에는 되도록이면 텍스트 링크를 사용할 것
* 페이지간의 이동을 텍스트 링크를 통해 하면 검색 엔진이 사이트를 크롤링하고 이해하기가 더 쉬워짐. 이미지같은걸 사용하면 검색엔진이 링크를 찾기 어려워진다.

# sitemap.xml
* google이 페이지를 쉽게 찾을 수 있게 해준다.
* google에게 선호하는 표준 URL을 알려줄 수 있다.

# 이미지 최적화
## alt 속성 활용하기
* 이미지를 링크로 상용하는 경우 alt 속성의 텍스트는 텍스트 링크의 앵커 텍스트와 유사하게 처리됨

# robot.txt
* 검색에 노출이 필요하지 않은 부분을 robots.txt로 제어
* 이름 변경 경 안 됨. 사이트의 루트 디렉토리에 있어야 됨
* robot.txt를 보안 도구로 사용하지 말것. 트래픽을 조절하기 위한 규약임.
* 가이드라인은 [여기](https://support.google.com/webmasters/answer/6062596?hl=ko&ref_topic=6061961)서

## robot.txt 사용 예시
* `/cgi-bin/`, `/tmp/`, `/~name/` 경로를 제외하고 허용
```
User-agent: *
Disallow: /cgi-bin/
Disallow: /tmp/
Disallow: /~name/
```

* 사이트 전체 허용
```
User-agent: *
Disallow:
```

* 검색엔진에서 사이트를 삭제하고 향후 어떤 검색봇도 접근하지 못하게 함
```
User-agent: *
Disallow: /
```

* 특정 파일(gif) 차단
```
User-Agent: Googlebot
Disallow: /*.gif$
```

* ?가 포함된 url 차단
```
User-agent: Googlebot
Disallow: /*?
```


# ref
* https://ko.wikipedia.org/wiki/%EA%B2%80%EC%83%89_%EC%97%94%EC%A7%84_%EC%B5%9C%EC%A0%81%ED%99%94
* https://static.googleusercontent.com/media/www.google.com/ko//intl/ko/webmasters/docs/search-engine-optimization-starter-guide-ko.pdf
* https://www.twinword.co.kr/blog/search-engine-optimization-guide/
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjQ0MjQzODY5XX0=
-->