---
layout: default
title: Vue with django
parent: vue
nav_order: 2
---

# Vue with django

### 1. Django REST framework + Vue.js\(Vue CLI\)

애초에 백엔드와 프론트엔드를 나눠서 작업한다. Vue.js는 Vue CLI를 사용해서 설치하는 것이 정신건강에 이롭다. 이건 처음부터 프로젝트 시작하는 경우에 적절하다.

### 2. Django + Vue.js 라이브러리

기존 Django 템플릿에 Vue.js 라이브러리를 포함시켜버리는 방법이다. 그냥 간단하게 Script 파일을 html 파일 내에 추가해도 되지만, 이렇게 Vue.js를 사용하면 싱글 파일 컴포넌트\(.vue\)를 사용할 수 없다. 참고로 뷰의 싱글 파일 컴포넌트는 별도의 컴파일 과정이 없으면 웹 브라우저가 인식할 수 없다.

### 3. Django + Vue.js\(Vue CLI\) + Webpack

Webpack으로 싱글 파일 컴포넌트\(.vue\) 파일을 자바스크립트 코드로 컴파일한 다음 빌드하는 방식이다. 이 방식으로 구축하면 기존 Django 프로젝트에 Vue.js의 싱글 파일 컴포넌트를 사용할 수 있다.

## References

* [https://www.reddit.com/r/django/comments/8qw2tt/any\_recommendations\_on\_how\_to\_integrate\_vuejs/](https://www.reddit.com/r/django/comments/8qw2tt/any_recommendations_on_how_to_integrate_vuejs/)
* [https://medium.com/@jrmybrcf/how-to-build-api-rest-project-with-django-vuejs-part-i-228cbed4ce0c](https://medium.com/@jrmybrcf/how-to-build-api-rest-project-with-django-vuejs-part-i-228cbed4ce0c)

