---
layout: default
title: 삽입 정렬
parent: 정렬 알고리즘
grand_parent: algorithm
nav_order: 3
---

# 삽입 정렬(Insertion sort)

## 1. 삽입 정렬이란

![삽입 정렬](https://t1.daumcdn.net/cfile/tistory/2569FD3854508BE811)

해당 위치의 수가 앞의 수보다 작으면 앞으로 보내버린다. 카드 게임에서 카드를 정렬하는 거랑 비슷하다.

### 2. 소스코드

```js
var arr = [4, 2, 3, 1, 5]

function swap(arr, index1, index2) {
  var temp = arr[index1];
  arr[index1] = arr[index2];
  arr[index2] = temp;
}

function insertionSort(arr) {
  var len = arr.length;
  var key, j;
  for (i = 1; i < len; i++) {
    key = arr[i];
    j = i - 1;

    while (j >= 0 && arr[j] > key) {
      arr[j + 1] = arr[j];
      j--;
    }
    arr[j + 1] = key;
  }
  return arr;
}

console.log(insertionSort(arr))
```

## References

* [정렬 - 삽입정렬](https://wonjayk.tistory.com/218)
