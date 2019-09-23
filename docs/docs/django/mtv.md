---
layout: default
title: 모델, 템플릿, 뷰 개요
parent: django
nav_order: 2
---

# MTV

- 장고는 MTV 기반의 프레임워크..
- 장고에서는 View를 템플릿, Controller를 View라고 부름
- 용어만 다를 뿐 MVC랑 개념은 동일

## Model

- 각 app의 Model.py
- 데이터베이스에 저장되어있는 데이터를 가지고 오거나 수정 등등
- ORM 기법임(객체를 사용해 디비 관리)
- 하나의 모델 클래스는 하나의 테이블에 매핑
- 모델 클래스의 속성은 테이블의 컬럼에 매핑
- 아무튼 이렇게 ORM 기법으로 매핑하면 앱에서는 SQL 없이도 클래스를 다룰 수 있어서 편리. 그리고 DB 엔진 변경 쉽게 할 수 있음.
- 테이블 및 컬럼을 자동으로 생성하기 위해서 많은 규칙을 가지고 있는데 [공식 문서](https://docs.djangoproject.com/en/2.1/topics/db/models/)에서 확인 가능

## Template

- templates 폴더 안에 위치
- 사용자에게 보여지는 부분
- settings.py 파일의 templates에서 경로 지정 가능

## View

- 각 app의 view.py.
- 실질적으로 프로그램 로직이 동작하여 데이터를 가져오고 적절하게 처리한 결과를 전달
- 버튼을 눌렀을 때 어떤 함수를 호출하여 데이터를 어떻게 가공할 것인지...

![mtv]({{site.url}}/TIL/assets/images/django/mtv/mtv.png)

1. 클라이언트로부터 요청을 바등면 URLconf 모듈을 이용해서 URL 분석
2. URL 분석 결과를 통해 URL을 처리할 뷰 결정
3. 뷰는 로직 실행하면서 디비 처리가 필요할 경우 모델을 통해 요청하고 그 값을 반환받음
4. 뷰는 로직 처리가 끝나면 템플릿을 사용하여 HTML 파일 생성
5. 뷰는 최종 결과로 HTML 파일을 클라이언트한테 보내 응답
