---
layout: default
title: 리액트에서 배열 다루기
parent: react
nav_order: 2
---

# 리액트에서 배열 다루기

보통 배열에 추가할 때 `push`메소드를 사용해서 데이터 추가를 하는데, 리액트에서는 그렇게 하면 안 된다. \(불변성 유지\) 즉, `state` 내부의 값을 직접적으로 수정하면 안 된다. `push`, `splice`, `unshift`, `pop` 같은 내장함수는 배열 자체를 직접 수정하게 되므로 적합하지 않다. 대신에 `concat`, `slice`, `map`, `filter` 같은 함수를 사용해야 한다.

