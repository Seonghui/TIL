---
layout: default
title: v-model
parent: vue
nav_order: 2
---

# v-model

## 기본 개념

* 폼 input과 textarea element에 양방향 데이터 바인딩을 생성. 참고로 v-model을 사용하면 form의 기본 속성들\(value, checked, selected\)등을 무시.

## 취소 기능 구현하기

* 양방향 데이터 바인딩 때문에 폼 취소 기능을 구현하고 싶어도 생각대로 안 된다. 왜냐면 폼 입력을 끝내고 취소해도 데이터가 실시간으로 바인딩되기 때문임.
* 해결하려면 여러 방법이 있는데, 폼에 value 값을 줘서 구현하는 방법도 있지만 cache mechanism을 이용해서도 구현할 수 있다.
* vue로 구현한 todomvc를 보니 비슷하게 cache를 이용해서 구현했음.
* 아무튼 cache mechanism의 대략적인 개념은, 수정 버튼을 누를 때 cache 변수에 수정하지 않은 데이터를 복사. 그리고 취소 버튼을 누를 때 현재 데이터에 cache 변수값을 집어넣는 방식임.

### References

* [Editing a form with save and cancel options](https://stackoverflow.com/questions/46166359/editing-a-form-with-save-and-cancel-options)
* [Vue.js return object to previous state on cancel](https://medium.com/@nickdenardis/vue-js-return-object-to-previous-state-on-cancel-2fa0f2db700a)
