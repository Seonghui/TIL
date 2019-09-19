---
layout: default
title: 선택 정렬
parent: 정렬 알고리즘
grand_parent: algorithm
nav_order: 2
---

# 선택 정렬(Selection sort)

## 1. 선택 정렬이란

선택 정렬(選擇整列, selection sort)은 제자리 정렬 알고리즘의 하나로, O(n^2)의 시간 복잡도를 가진다. 말 그대로 배열에서 아이템을 선택해 맨앞으로 보낸다. 따라서 앞부터 정렬이 된다. 선택 정렬은 다음과 같은 순서로 이루어진다.

![https://hudi.kr](https://hudi.kr/wp-content/uploads/2018/02/selectionsort.gif)

1. 주어진 리스트 중에 최소값을 찾는다.
2. 그 값을 맨 앞에 위치한 값과 교체한다(패스(pass)).
3. 맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.

## 2. 예제

| 패스 | 테이블 | 최솟값 |
| :--- | :--- | :--- |
| 0 | [**9,1,6,8,4,3,2,0**] | 0 |
| 1 | [0,**1,6,8,4,3,2,9**] | 1 |
| 2 | [0,1,**6,8,4,3,2,9**] | 2 |
| 3 | [0,1,2,**8,4,3,6,9**] | 3 |
| 4 | [0,1,2,3,**4,8,6,9**] | 4 |
| 5 | [0,1,2,3,4,**8,6,9**] | 6 |
| 6 | [0,1,2,3,4,6,**8,9**] | 8 |

## 3. 의사코드

```text
for i = 0 to n:
    a[i]부터 a[n - 1]까지 차례로 비교하여 가장 작은 값이 a[j]에 있다고 하자.
    a[i]와 a[j]의 값을 서로 맞바꾼다.
```

## 4. 소스코드

### 4-1. python

```python
def selection_sort(arr):
    length = len(arr)
    for i in range(length - 1):
        min_idx = i
        for j in range(i + 1, length):
            if arr[min_idx] > arr[j]:
                min_idx = j
        # swap
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr


arr = [9, 1, 6, 8, 4, 3, 2, 0]
selection_sort(arr)
print(arr)
```

### 4-2. javascript

```javascript
var arr = [4, 2, 3, 1, 5];

function swap(arr, index1, index2) {
  var temp = arr[index1];
  arr[index1] = arr[index2];
  arr[index2] = temp;
}

function selectionSort(arr) {
  var len = arr.length;
  for (var i = 0; i < len - 1; i++) {
    var min = i;
    for (var j = i + 1; j < len; j++) {
      if (arr[j] < arr[min]) {
        min = j;
      }
    }
    swap(arr, min, i);
  }
  return arr;
}

console.log(selectionSort(arr));
```

## 참고 자료

* [위키백과 - 선택정렬](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%83%9D_%EC%A0%95%EB%A0%AC)
