# 분할 정복 (Divide and Conquer)

## 분할 정복 알고리즘의 개념

분할 정복은 두 가지 단계를 거친다.

1. 기본 단계를 해결한다. 이 부분은 가능한 간단한 문제여야 한다.
2. 문제가 기본 단계가 될 때까지 나누거나 작게 만든다.

## 2. 분할 정복 알고리즘의 종류

1. 합병 정렬 (Merge Sort)
2. 퀵정렬 (Quick Sort)

### 2-1. 합병 정렬 (Merge Sort) - O(n log n)

![https://www.techiedelight.com/merge-sort/](https://i2.wp.com/www.techiedelight.com/wp-content/uploads/Merge-Sort-Steps.png?zoom=2&resize=389%2C368&ssl=1)

1. 쪼개고 쪼개(분할) 가장 작은 단위에서부터 시작한다.
2. 합쳐질 때(정복) 정렬이 된다.

#### 2-1-1. 소스 코드 (Javascript)

```javascript
var arr = [4, 2, 3, 1, 5];

function mergeSort(arr) {
  var len = arr.length;
  if (len === 1) {
    return arr;
  }

  var middle = Math.floor(len / 2);
  var left = arr.slice(0, middle);
  var right = arr.slice(middle, len);

  function merge(left, right) {
    var result = [];
    while (left.length && right.length) {
      if (left[0] <= right[0]) { // 두 배열의 첫 원소를 비교해서
        result.push(left.shift()); // 더 작은 수를 결과에 넣어준다
      } else {
        result.push(right.shift()); // 오른쪽도 마찬가지
      }
    }
    while (left.length) result.push(left.shift()); // 어느 한 배열이 더 많이 남았다면 나머지를 다 넣는다
    while (right.length) result.push(right.shift()); // 오른쪽도 마찬가지
    return result;
  };
  return merge(mergeSort(left), mergeSort(right));
}

console.log(mergeSort(arr));
```

### 2-2. 퀵정렬 (Quick Sort) - O(n log n)

1. 피벗 기준, 피벗보다 작으면 왼쪽, 피벗보다 크면 오른쪽으로 보내버린다.
2. 다 보내면 그 피벗을 왼쪽과 오른쪽의 중간으로 보내버린다.
3. 피벗을 제외한 왼쪽 리스트와 오른쪽 리스트를 다시 정렬한다
4. 그 리스트가 0이나 1이 될 때까지 반복한다.

#### 2-2-1. 소스 코드 (Javascript)

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
