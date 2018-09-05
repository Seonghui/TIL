이진 탐색(Binary Search)
===
> 1부터 100까지의 숫자 중 하나(target)를 맞춰야 하는 문제가 있다.

## 단순 탐색 (n)
* 순서대로 모두 추측한다.
* 답이 99일 경우 99번 추측한다.
* 추측해야 할 최대 횟수는 리스트의 길이와 같은데, 이런 것을 선형 시간이고 한다. (n)

## 이진 탐색 (log2n)
* 차례대로 찾는 게 아니라 중간 어디쯤에서 찾는 것을 이진 탐색(Binary)이라고 한다.
* 이진 탐색 알고리즘은 리스트에 원하는 원소가 있으면 그 원소의 위치를 반환하고, 아니면 null을 반환한다.
* 이진 탐색은 로그 시간으로 실행된다. (log2n)
* 중간부터 탐색한다. 업앤다운 게임을 생각하자.
* 답이 1인 경우 탐색은 100 > 50 > 25 > 13 > 7 > 4 > 2 > 1이 된다.
* 이진 탐색은 순서대로 정렬이 되어있어야 한다. [34, 12, 64, 34, 17] 이런 배열에서 이진 탐색은 불가능하다.

```javascript
const arr = [1, 3, 5, 7, 9, 11, 12, 64];
const binarySearch = ((list, target) => {
    let mid;
    let start = 0;
    let end = list.length - 1;

    while (start <= end) {
        mid = Math.floor((start + end) / 2);

        if (list[mid] === target) {
            return mid;
        }

        if (list[mid] > target) {
            end = mid - 1;
        }
                
        if  (list[mid] < target) {
            start = mid + 1;
        }
    }
    return -1;
});

binarySearch(arr, 12);
```