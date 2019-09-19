---
layout: default
title: 퀵 정렬
parent: 정렬 알고리즘
grand_parent: algorithm
nav_order: 5
---

# 퀵 정렬(Quick sort)

## 분할 정복 알고리즘의 개념

분할 정복은 두 가지 단계를 거친다.

1. 기본 단계를 해결한다. 이 부분은 가능한 간단한 문제여야 한다.
2. 문제가 기본 단계가 될 때까지 나누거나 작게 만든다.

## 분할 정복 알고리즘의 종류

1. 합병 정렬 (Merge Sort)
2. 퀵정렬 (Quick Sort)

여기서는 퀵정렬에 대해 다룬다.

### 퀵정렬 (Quick Sort) - O(n log n)

1. 피벗 기준, 피벗보다 작으면 왼쪽, 피벗보다 크면 오른쪽으로 보내버린다.
2. 다 보내면 그 피벗을 왼쪽과 오른쪽의 중간으로 보내버린다.
3. 피벗을 제외한 왼쪽 리스트와 오른쪽 리스트를 다시 정렬한다
4. 그 리스트가 0이나 1이 될 때까지 반복한다.

#### 소스 코드 (Javascript)

```javascript
var arr = [4, 2, 3, 1, 5];

function quickSort(arr) {
  if (arr.length === 0) {
    return [];
  }

  var middle = arr[0];
  var len = arr.length;
  var left = [], right = [];

  for (var i = 1; i < len; i++) {
    if (arr[i] < middle) {
      left.push(arr[i]);
    } else {
      right.push(arr[i]);
    }
  }

  return quickSort(left).concat(middle, quickSort(right));
}

console.log(quickSort(arr));
```
