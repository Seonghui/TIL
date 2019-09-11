---
layout: default
title: 웹 이미지 형식
parent: etc
nav_order: 2
---

# 웹 이미지 형식

![&#xC774;&#xBBF8;&#xC9C0; &#xD3EC;&#xB9F7;](https://i.stack.imgur.com/Ow8RZ.png)

full-width 슬라이드쇼 이미지는 최대 가로 2560px 이미지가 괜찮음. 대다수 27, 30인치의 모니터는 가로가 2560px

### png

* 또렷하고 선명한 이미지에 좋음
* 대신 색이 적을수록 좋다
* 주로 레이아웃을 위한 이미지에 사용 \(재사용할 일이 많은 이미지에 사용하기 좋음\)
* 애니메이선 지원 안 됨

png-8과 png-24가 있음

* png-8: indexed color 속성으로 최대 256가지의 컬러로만 저장
* png-24: jpg처럼 컬러를 많이 가지지만 여전히 무손실 압축임. 주로 간단한 배너이미지에 사용

### jpg

* 손실 압축
* low-bandwidth 이미지
* 별로 안 선명함
* 주로 이미지에 사용 \(특정 페이지에서 보여주는 크고 복잡한 이미지에 사용하기 좋음\)
* 퀄리티는 최대 퀄리티의 60~70%로 export해도 충분함. 만약 50~60%까지 떨어지는 경우 육안으로 노이즈가 보임.

### gif

* 무손실 압축 지원
* indexed color 속성으로 최대 256가지의 컬러로만 저장
* 색이 별로 없어서 이미지에 좋은 형식이 아니다
* 애니메이션 가능

### svg

* vector 파일임
* 픽셀 대신 라인과 곡선으로 이루어져있음
* 로고나 아이콘에 적합
* 모양을 최대한 단순화하는것이 최적화하기 좋음. 안 그러면 크기가 매우커진다.

### png/svg/gif 공통점

![export file size comparison](https://cdn.foregroundweb.com/wp-content/uploads/2016/03/adobe-lightroom-jpg-export-file-size-comparison.png)

solid color를 보여주기에 좋음. 예를 들면 로고, 아이콘 같은.. 만약 컬러가 많은 이미지를 위의 포맷으로 저장하는 경우 용량이 엄청나게 커짐

## 이미지 최적화 툴

* gif: [http://www.lcdf.org/gifsicle/](http://www.lcdf.org/gifsicle/)
* jpg: [http://jpegclub.org/jpegtran/](http://jpegclub.org/jpegtran/)
* 무손실 png: [http://optipng.sourceforge.net/](http://optipng.sourceforge.net/)
* 손실 png: [https://pngquant.org/](https://pngquant.org/)

## refs

* [https://www.foregroundweb.com/image-size/](https://www.foregroundweb.com/image-size/)
* [https://stackoverflow.com/questions/1698330/webdesign-jpg-or-png-which-one-is-the-best-for-web](https://stackoverflow.com/questions/1698330/webdesign-jpg-or-png-which-one-is-the-best-for-web)
* [https://medium.com/@soeunlee/web%EC%97%90%EC%84%9C-png-gif-jpeg-svg-%EC%A4%91-%EC%96%B4%EB%96%A4-%EA%B2%83%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%A9%B4-%EC%A2%8B%EC%9D%84%EA%B9%8C%EC%9A%94-6937300e776e](https://medium.com/@soeunlee/web%EC%97%90%EC%84%9C-png-gif-jpeg-svg-%EC%A4%91-%EC%96%B4%EB%96%A4-%EA%B2%83%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%A9%B4-%EC%A2%8B%EC%9D%84%EA%B9%8C%EC%9A%94-6937300e776e)

