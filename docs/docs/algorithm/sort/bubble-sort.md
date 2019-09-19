---
layout: default
title: 버블 정렬
parent: 정렬 알고리즘
grand_parent: algorithm
nav_order: 1
---

# 버블 정렬(Bubble Sort)

## 1. 버블 정렬이란

![버블 정렬](https://www.w3resource.com/w3r_images/bubble-short.png)

인접한 두 수를 비교해서 큰 수를 뒤로 보낸다. 뒤부터 정렬이 된다.

## 2. 소스코드

```js
var arr = [4, 2, 3, 1, 5]

// 헬퍼 함수
// O^2
function swap(arr, index1, index2) {
  var temp = arr[index1];
  arr[index1] = arr[index2];
  arr[index2] = temp;
}

// 1. 버블 정렬
// O^2
function bubbleSort(arr) {
  var len = arr.length;
  for (var i = 0; i < len - 1; i++) {
    for (var j = 0; j < len - i - 1; j++) {
      // len - i - 1인 이유는, 뒷부분은 이미 정렬이 된 상태이기 때문
      if (arr[j] > arr[j + 1]) {
        swap(arr, j, j + 1);
      }
    }
  }
  return arr;
}

console.log(bubbleSort(arr))
```

## References

* [C# Sharp Searching and Sorting Algorithm Exercises: Bubble sort](https://www.w3resource.com/csharp-exercises/searching-and-sorting-algorithm/searching-and-sorting-algorithm-exercise-3.php)
