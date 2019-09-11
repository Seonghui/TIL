---
layout: default
title: 패키지 관리자(Package Manager)
parent: etc
nav_order: 2
---

## 1. npm \(Node Package Manager\)

자바스크립트 프로그래밍 언어를 위한 패키지 관리자. node.js로 만들어진 모듈이나 패키지 관리가 가능. 비유를 하자면 자바스크립트\(node.js\)계의 앱스토어\(?\)라고 생각하면 됨. 아무튼 기존의 모듈들을 사용해서 적절하게 활용하면 개발을 편하게 할 수 있음. \(ex, react.js용 음력 달력 모듈, react.js용 파일 업로드 모듈 등\). npm init 명령어를 입력하면 프로젝트 관리를 위한 package.json 파일 생성이 됨.

### 명령어 모음

```text
# package.json에 있는 모든 디펜던시 설치하기
npm install

# 개발용 디펜던시 설치
npm install <패키지 이름> --save-dev

# 특정 버전 설치
npm install <패키지 이름>@버전

# 전역으로 설치
npm install <패키지 이름> -g

# 중복 패키지 정리
npm dedupe

# 설치한 패키지 조회 (매우 많으니 그냥 package.json 파일 확인하기)
npm ls

# 오래된 패키지 조회
npm outdated
```

## 2. yarn

새로운 패키지 매니저임. npm보다 빠르고 안정적이고 보안성이 뛰어나다고 주장하고 있음. npm을 통해 설치할 수 있음.
