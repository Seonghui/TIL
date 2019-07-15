# responsive-images

### 반응형 이미지

* 장식용 이미지는 css 배경 이미지로 구현

## srcset, sizes

* 브라우저가 올바른 사진을 선택할 수 있게 함

  ```text
  <img srcset="elva-fairy-320w.jpg 320w,
             elva-fairy-480w.jpg 480w,
             elva-fairy-800w.jpg 800w"
     sizes="(max-width: 320px) 280px,
            (max-width: 480px) 440px,
            800px"
     src="elva-fairy-800w.jpg" alt="요정 옷을 입은 엘바">
  ```

### srcset

브라우저에게 제시할 이미지 목록과 크기를 정의. 각 쉼표 앞에 아래와 같이 적음

1. 이미지 파일명
2. 공백
3. 이미지 고유 픽셀 너비 \(w단위를 사용해야 한다. 맥의 경우 cmd+i를 눌러 확인 가능\)

### size

미디어 조건문들을 정의\(화면 크기 등\)하고 특정 미디어 조건문이 참일 때 어떤 이미지 크기가 최적인지 나타낸다. 이 경우, 각 쉼표 전에 이렇게 쓴다.

1. 미디어 조건문
2. 공백
3. 미디어 조건문이 참일 경우 이미지가 채울 슬롯의 너비 \(너비로는 절대값이나 상대값을 넣어야 한다. 절대값은 px, em이고 상대값의 예시로는 vw이 있다. 그런데 퍼센트는 불가능!!\)

### 동작

속성들을 제대로 적으면 브라우저는 아래와 같이 함. 1. 기기 너비 확인하기 2. sizes 목록에서 가장 먼저 참이 되는 미디어 조건문을 확인 3. 해당 미디어 쿼리가 제공하는 슬롯 크기를 확인 4. 해당 슬롯 크기에 가장 근접하게 맞는 srcset에 연결된 이미지를 불러옴

## 다양한 디스플레이 해상도에서 동일한 이미지 사이즈 보여주기

srcset에 sizes 없이 x-서술자 사용하기

```text
<img srcset="elva-fairy-320w.jpg,
            elva-fairy-480w.jpg 1.5x,
            elva-fairy-640w.jpg 2x"
    src="elva-fairy-640w.jpg" alt="요정 옷을 입은 엘바">
```

## picture

* srcset/size 속성만으로 해결할 수 없는 문제들 \(아트 디렉션 처리, 여러 이미지 포맷\)은 picture로 해결. 
* img 태그를 넣는 이유는 picture 요소를 지원하지 않는 브라우저에 대체제를 제공
* 아트 디렉션이 필요한 경우가 아니라면 media 속성 사용하지 말자 \(여기서 아트 디렉션이란 이미지의 의도가 제대로 전달되도록 기기에 따라 사진의 핵심을 확대해서 보여주거나 잘라서 보여주는 방식임\)
* `<source>` 요소에서, type에 유형을 선언한 이미지만 사용할 수 있음

```text
<picture>
  <source media="(max-width: 799px)" srcset="elva-480w-close-portrait.jpg">
  <source media="(min-width: 800px)" srcset="elva-800w.jpg">
  <img src="elva-800w.jpg" alt="딸 엘바를 안고 서 있는 크리스">
</picture>
```

* source media: 어떤 이미지를 보여줄지 결정하는데 사용
* srcset: 보여줄 이미지 경로

## 알아둘 것

IE 미지원

## refs

[https://developer.mozilla.org/ko/docs/Learn/HTML/Multimedia\_and\_embedding/](https://developer.mozilla.org/ko/docs/Learn/HTML/Multimedia_and_embedding/)

