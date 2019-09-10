---
layout: default
title: 이진 탐색
parent: algorithm
nav_order: 2
---

# 이진 탐색\(Binary Search\)

1부터 100까지의 숫자 중 하나\(target\)를 맞춰야 하는 문제가 있다.

## 1. 단순 탐색 \(n\)

* 순서대로 모두 추측한다.
* 답이 99일 경우 99번 추측한다.
* 추측해야 할 최대 횟수는 리스트의 길이와 같은데, 이런 것을 선형 시간이고 한다. \(n\)

## 2. 이진 탐색 \(log2n\)

* 차례대로 찾는 게 아니라 중간 어디쯤에서 찾는 것을 이진 탐색\(Binary\)이라고 한다.
* 이진 탐색 알고리즘은 리스트에 원하는 원소가 있으면 그 원소의 위치를 반환하고, 아니면 null을 반환한다.
* 이진 탐색은 로그 시간으로 실행된다. \(log n\)
* 중간부터 탐색한다. 업앤다운 게임을 생각하자.
* 답이 1인 경우 탐색은 100 &gt; 50 &gt; 25 &gt; 13 &gt; 7 &gt; 4 &gt; 2 &gt; 1이 된다.
* 이진 탐색은 순서대로 정렬이 되어있어야 한다. \[34, 12, 64, 34, 17\] 이런 배열에서 이진 탐색은 불가능하다.

## 3. 예제

```python
def binary_search(list, item):
  low = 0
  high = len(list) - 1

  # 탐색 범위를 하나로 줄이지 못했으면 계속 실행
  while low <= high:
    # 가운데 숫자를 확인
    mid = (low + high) // 2
    guess = list[mid]
    # 아이템을 찾았다면
    if guess == item:
      return mid
    # 추측한 숫자가 너무 크다면
    if guess > item:
      high = mid - 1
    # 추측한 숫자가 작다면
    else:
      low = mid + 1

  # 아이템이 존재하지 않는다면
  return None

my_list = [1, 3, 5, 7, 9]

print (binary_search(my_list, 3)) # => 1
print (binary_search(my_list, -1)) # => None
```

## References

* 그림으로 개념을 이해하는 알고리즘

