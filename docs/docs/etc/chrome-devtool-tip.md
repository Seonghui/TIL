---
layout: default
title: 크롬 개발자도구 - 팁
parent: etc
nav_order: 2
---

# 크롬 개발자 도구 - 팁

전체 웹사이트가 아닌 해당 페이지에서의 사용 여부를 나타내줌

### 1. `Command+Shift+P` 눌러 커맨드창 열고 Coverage 타이핑해서 Show Coverage 클릭

![coverage](https://developers.google.com/web/tools/chrome-devtools/speed/imgs/commandmenu.png?hl=ko)

### 2. reload 버튼 클릭해서 리로드하면 파일을 얼마나 안사용하는지 알 수 있음

![coverage](https://developers.google.com/web/tools/chrome-devtools/speed/imgs/coveragereport.png?hl=ko)

### 3. 파일 클릭하면 사용하는건 초록색, 안사용하는건 빨간색으로 나옴

![coverage](https://developers.google.com/web/tools/chrome-devtools/speed/imgs/jquery.png?hl=ko)

## 크롬 개발자도구에서 특정 파일 block하고 보기

### 1. 네트워크 탭 클릭

![blocking](https://developers.google.com/web/tools/chrome-devtools/speed/imgs/blocking.png?hl=ko)

### 2. `Command+Shift+P` 눌러 커맨드창 열고 block 검색해서 show request blocking 선택

![blocking](https://developers.google.com/web/tools/chrome-devtools/speed/imgs/blocking.png?hl=ko)

### 3. + 버튼 누르고 특정 파일 경로나 파일명 추가하고 엔터 \(예제에서는 `/libs/*`\)

![blocking](https://developers.google.com/web/tools/chrome-devtools/speed/imgs/libs.png?hl=ko)

### 4. 리로드하면 blocking한 파일들은 빨간색으로 표시됨.

![blocking](https://developers.google.com/web/tools/chrome-devtools/speed/imgs/blockedlibs.png?hl=ko)

## refs

[https://developers.google.com/web/tools/chrome-devtools/speed/get-started?hl=ko](https://developers.google.com/web/tools/chrome-devtools/speed/get-started?hl=ko)

