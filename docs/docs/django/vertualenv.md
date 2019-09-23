---
layout: default
title: 가상 환경 세팅하기
parent: django
nav_order: 2
---

# 가상환경(Virtual environment)

> 가상환경(Virtual environment), 줄여서 virtualenv

독립적인 python 가상 환경을 새로 설정해 다른 프로젝트를 진행하면서 설치했던 라이브러리들간의 의존성 문제를 해결할 수 있도록 해줌. 즉 격리된 별도의 라이브러리 설치 디렉토리를 뜻함. 가상환경은 편의성을 위해 프로젝트별로 하나씩 생성하는 것이 좋다.

## 파이썬 가상환경 라이브러리

### virtualenv

python2에서는 가상환경 라이브러리가 기본 제공되지 않아 써드파티 라이브러리인 virtualenv 라이브러리가 필수임. (하지만 python3에서는 vevn라는 가상환경 라이브러리가 기본 제공되기 때문에 굳이 쓰지 않아도 됨)

### venv

python3 기본 제공 라이브러리임. virtualenv와 비교해서 가상환경 생성 명령만 다를 뿐, 활성화, 설치, 비활성화 부분은 동일함.

## How To Use(venv)

### 1. 가상환경 만들기

**myvenv**라는 이름의 가상환경 만들기.

```bash
python3 -m venv myvenv
```

### 2. 가상환경 실행하기

```bash
source myvenv/bin/activate
```

만약 **source를 사용할 수 없는 경우** 아래와 같이 입력하기

```bash
. myvenv/bin/activate
```

입력하면 콘솔의 프롬포트 앞에 (myvenv) 접두어가 붙는다. 가상환경 활성화 이후 작업할 폴더로 이동해 장고를 설치하면 된다.

### 3. 종료하기

```bash
deactivate
```

### References

* [djangogirls](https://tutorial.djangogirls.org/ko/installation/)
* [파이썬 웹프로그래밍 실전편](https://medium.com/%EB%8F%84%EC%84%9C-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%9B%B9%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%8B%A4%EC%A0%84%ED%8E%B8-%EC%9A%94%EC%95%BD/chapter-6-%EA%B0%80%EC%83%81-%ED%99%98%EA%B2%BD-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-%EC%83%88%EB%A1%AD%EA%B2%8C-%EC%A0%95%EB%A6%AC-30d5940de012)

