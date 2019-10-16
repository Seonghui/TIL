---
layout: default
title: 트리
parent: 자료구조
grand_parent: algorithm
nav_order: 4
---

# 트리
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

![트리](https://splsxw.bn.files.1drv.com/y4mnUrYeYEFR94eta575YePHkHKC6QoTmQ8MKgXSR6c7zBmKIHbE-dipCj6cMWTR5wpehsRuLgFZPAkMkBXXlkf2w21iLpiGuwMWn7yTfJsi4QgBl0eR6oF22plrXcFDEom5BCLh0qKDrD1yP1j2QbR_r0SFKKA-hlhG40iQPJq599TnyS5gjEPBsT3ev0HN9FW0id3aWXfcS32myddiufm3w?width=1496&height=902&cropmode=none)

임의의 노드에서 다른 노드로의 경로가 하나 밖에 없는 것을 트리 자료구조라고 한다. 트리 자료구조는 노드 중에서 단 하나의 루트 노드(Root Node)가 있고, 루트 노드에서 하위 노드(Sub Node)들이 연결된 비선형 계층 구조이다. 트리 구조 중에서 다음 데이터를 가리키는 것이 2개인 단방향 트리 구조를 이진 트리 구조라고 한다.

트리 자료구조의 구현은 배열이나 연결 리스트를 사용한다.

## 관련 용어

| 용어 | 설명 |
| :--- | :--- |
| 노드(node) | 트리를 구성하는 기본 원소 |
| 루트 노드(root node/root) | 트리에서 부모가 없는 최상위 노드, 트리의 시작점 |
| 부모 노드(parent node) | 루트 노드 방향으로 직접 연결된 노드 |
| 자식 노드(child node) | 루트 노드 반대방향으로 직접 연결된 노드 |
| 형제 노드(siblings node) | 같은 부모 노드를 갖는 노드들 |
| 리프 노드(leaf node/leaf/terminal node) | 자식이 없는 노드 |
| 경로(path) | 한 노드에서 다른 한 노드에 이르는 길 사이에 있는 노드들의 순서 |
| 길이(length) | 출발 노드에서 도착 노드까지 거치는 노드의 갯수 |
| 깊이(depth) | 루트 경로의 길이 |
| 레벨(level) | 루트 노드(1) 로 부터 노드까지 연결된 엣지의 수의 합 |
| 높이(height) | 가장 긴 루트 경로의 길이 |
| 차수(degree) | 각 노드의 자식의 갯수 |
| 트리의 차수(degree of tree) | 트리의 최대 차수 |
| 크기(size) | 노드의 개수 |
| 너비(width) | 가장 많은 노드를 갖고 있는 레벨의 크기 |

## 이진 트리 (Binary Tree)

![이진 트리](https://ffmdgg.bn.files.1drv.com/y4mGn4aiXr6PbhAIzoDz_rADNiULJDFClavfPV3lYFPsBRhvKpv-REegUqCjEzd7-7iv8OO7FPWUTnX1fl5WrtKO7DZW3oiZFbNpIp_1FQ0YGg4bOyP8sDQ7Bx-FPCbkqQRhm62AJwEE1GpDsmqAnRPGmpsTqsS8eD5khXWdQZuukfUJTav0mZeStOW1jUFmVX-bk3bSIYGC2clWzLkmL7-Vw?width=882&height=642&cropmode=none){:height="50%" width="50%"}

이진 트리 구조는 트리 자료구조 중에서 모든 노드가 최대 2개의 자식 노드를 가질 수 있는 구조를 말한다. 왼쪽 서브 트리의 값은 루트의 값보다 작고, 오른쪽 서브 트리의 값은 루트보다 큰 값을 가지도록 구성한다.

### 이진 트리의 분류

이진 트리를 구성하다 보면 여러 가지 모양이 나올 수 있으며, 모양에 따라 3가지로 분류할 수 있다.

* 포화 이진 트리(Full Binary Tree): 레벨의 노드가 꽉 차 있는 트리
* 완전 이진 트리(Complete Binary Tree): 마지막 레벨 전까지는 노드가 꽉 차 있고, 마지막 레벨의 왼쪽에서 오른쪽으로 노드가 채워져 있는 트리
* 편향 이진 트리(Skewed Binary Tree): 왼쪽 혹은 오른쪽 서브 트리만을 가지는 트리

### 이진 트리의 순회 방식

![이진 트리의 순회방식](https://upload.wikimedia.org/wikipedia/commons/thumb/6/67/Sorted_binary_tree.svg/500px-Sorted_binary_tree.svg.png)

순회 방식은 자기 자신의 처리 순서에 따라 전위/중위/후위 3가지로 나눈다.

#### 전위(Preorder)

**자기 자신 -> 왼쪽 자식 -> 오른쪽 자식** 순으로 순회를 한다. 전위 순회는 깊이 우선 순회(depth-first traversal)라고도 한다.

위 트리를 전위 순회하면 F, B, A, D, C, E, G, I, H (root, left, right) 순으로 순회된다.

```text
preorder(node)
  print node.value
  if node.left ≠ null then preorder(node.left)
  if node.right ≠ null then preorder(node.right)
```

#### 중위(Inorder)

**왼쪽 -> 자기 자신 -> 오른쪽 자식** 순으로 순회를 한다. 중위 순회는 대칭 순회(symmetric)라고도 한다.

위 트리를 중위 순회하면 A, B, C, D, E, F, G, H, I (left, root, right) 순으로 순회된다.

```text
inorder(node)
  if node.left  ≠ null then inorder(node.left)
  print node.value
  if node.right ≠ null then inorder(node.right)
```

#### 후위(Postorder)

**왼쪽 자신 -> 오른쪽 자식 -> 자기 자신** 순으로 순회를 한다.

위 트리를 후위 순회하면 A, C, E, D, B, H, I, G, F (left, right, root) 순으로 순회된다.

```text
postorder(node)
  if node.left  ≠ null then postorder(node.left)
  if node.right ≠ null then postorder(node.right)
  print node.value
```

### 이진 트리의 입력, 검색, 삭제 과정

![이진 트리 사용](https://rxa1pq.bn.files.1drv.com/y4mkThF_SWiFacdW3FJD5XsA4iqfcgOg5257B3FU8BlbA6mG_MniqGlZ8Al9z5D3XCQiKY3T0GcniGFqMcLrrYkdT2yfQ6T9h5kRXmru6XmtEbvk5658YUc6Aq7d8LcYTUX9FUcvIxlkFMDgJeQtFwOcqihzXtFTavYGajInzgHUVQ07K7kuqkNzJOCVFYnDqJx_GOMKRHXM8JaS7FhVw_fDg?width=882&height=922&cropmode=none){:height="50%" width="50%"}

#### 입력

이진 트리에 7이 새롭게 입력되는 경우,

1. 8보다 작으므로 왼쪽
2. 3보다 크므로 오른쪽
3. 5보다 크므로 오른쪽에 위치시킨다.

#### 검색

이진 트리에서 9를 찾는 경우,

1. 8보다 크므로 오른쪽
2. 10보다 작으므로 왼쪽

#### 삭제

이진 트리에서 10을 삭제하는 경우

1. 9가 10의 자리로 올라간다.
2. 19가 9보다 크므로 9의 오른쪽에 위치한다.

만약 8을 삭제하는 경우에는 특정 노드를 후계자로 임의 선정하여 8의 위치에 놓고 나머지 노드를 앞에서 설명한 방법에 의해 재배치한다.

### 이진 트리 구현(Javascript)

| 메서드 | 설명 |
| :--- | :--- |
| insert(data) | 트리에 데이터 추가(노드 추가) |
| delete(data) | 노드 삭제 |
| search(data) | 노드 찾기 |
| getParent(node) | 부모 노드 반환 |
| getMin(node) | 트리의 최소값 반환 |
| getMax(node) | 트리의 최대값 반환 |
| preOrder(node) | 전위 순회 |
| inOrder(node) | 중위 순회 |
| postOrder(node) | 후위 순회 |

```js
function Node(data, left, right) {
  this.data = data;
  this.left = left;
  this.right = right;
}

function BST() {
  this.root = null;
}

BST.prototype.insert = function (data) {
  var node = new Node(data, null, null);
  // 만약 빈 트리라면
  if (this.root === null) {
    this.root = node;
  } else {
    var current = this.root;
    var parent;

    while (true) {
      parent = current;
      // 데이터가 현재 노드의 데이터보다 작을 경우
      if (data < current.data) {
        current = current.left;
        // 현재 노드의 데이터가 null일 경우
        if (current === null) {
          // 현재 위치에 노드 삽입
          parent.left = node;
          break;
        }
      } else {
        current = current.right;
        if (current === null) {
          parent.right = node;
          break;
        }
      }
    }
  }
}

BST.prototype.delete = function (data) {
  var current = this.search(data);
  var parent = this.getParent(current);

  if (current.right === null && current.left === null) { // 자식이 없는 노드를 지울 떄
    // 노드를 null로 만든다
    if (parent.left.data === data) {
      parent.left = null;
    } else {
      parent.right = null;
    }
  } else if (current.right !== null && current.left !== null) { // 자식이 둘 다 있을 때
    // 오른쪽 자식 중에서 가장 작은 값을 구한다
    var min = this.getMin(current.right);
    // 최소값을 복사한다
    var tempNode = min;
    // 최소값의 부모 노드를 구한다
    var minParent = this.getParent(min);

    // 최소값 노드와 부모 노드간의 연결을 끊는다
    if (minParent.right.data === min.data) {
      minParent.right = null;
    } else {
      minParent.left = null;
    }

    // 지우려고 하는 데이터와 최소값 노드의 데이터를 교체한다
    current.data = tempNode.data;
    // 최소값 노드에 자식노드를 연결해 준다
    current.right = tempNode.right;
  } else { // 자식이 하나만 있을 때
    // 삭제하려는 노드의 자식노드를 구한다
    var child = current.left === null ? current.right : current.left;
    // 삭제하려는 노드의 부모노드와 삭제하려는 노드의 자식노드를 연결한다
    if (parent.left == current) {
      parent.left = child;
    } else {
      parent.right = child;
    }
  }
}

BST.prototype.search = function (data) {
  var current = this.root;
  while (current.data !== data) {
    if (current.data > data) {
      current = current.left;
    } else {
      current = current.right;
    }
  }
  return current;
}

BST.prototype.getParent = function (node) {
  var current = this.root;
  var parent;

  while (current.data !== node.data) {
    parent = current;

    if (node.data < current.data) {
      current = current.left;
    } else {
      current = current.right;
    }
  }
  return parent;
}

BST.prototype.getMin = function (node) {
  var current = node;
  while (current.left !== null) {
    current = current.left;
  }
  return current;
}

BST.prototype.getMax = function (node) {
  var current = node;
  while (current.right !== null) {
    current = current.right;
  }
  return current;
}

// 전위
BST.prototype.preOrder = function (node) {
  console.log(node.data);
  if (node.left !== null) {
    this.preOrder(node.left);
  }
  if (node.right !== null) {
    this.preOrder(node.right);
  }
}

// 중위
BST.prototype.inOrder = function (node) {
  if (node.left !== null) {
    this.inOrder(node.left);
  }
  console.log(node.data);
  if (node.right !== null) {
    this.inOrder(node.right);
  }
}

// 후위
BST.prototype.postOrder = function (node) {
  if (node.left !== null) {
    this.postOrder(node.left);
  }
  if (node.right !== null) {
    this.postOrder(node.right);
  }
  console.log(node.data);
}

var tree = new BST();
tree.insert('F');
tree.insert('B');
tree.insert('G');
tree.insert('A');
tree.insert('D');
tree.insert('I');
tree.insert('C');
tree.insert('E');
tree.insert('H');

// tree.preOrder(tree.root); // F, B, A, D, C, E, G, I, H
// tree.inOrder(tree.root); // A, B, C, D, E, F, G, H, I
// tree.postOrder(tree.root); // A, C, E, D, B, H, I, G, F

tree.delete('D')
// tree.inOrder(tree.root); // A, B, C, E, F, G, H, I
tree.delete('F');
tree.inOrder(tree.root); // A, B, C, E, G, H, I
```

## 힙 (Heap)

여러 개의 값 중에서 가장 큰 값이나 가장 작은 값을 빠르게 찾을 수 있도록 구성된 자료구조이다. 힙은 배열을 사용하여 구현하는 것이 일반적이다.

* 최소 힙(Min Heap): 부모 노드의 값이 항상 하위 노드의 값보다 작은 경우
* 최대 힙(Max Heap): 부모 노드의 값이 항상 하위 노드의 값보다 큰 경우

### 최대 힙의 모양

![최대 힙의 모양](https://rjunmg.bn.files.1drv.com/y4mva15dB-3XbZO3D1-LV4cY_O5N96-wnVWD5zmNMcLA1xmfOphcOCBw_EnDZGr-j6TsjbrPaxLDYD1aXjVWFYH0FuZbsOdvH2DjKTCE71XlxSNVcT5VQyYGX7mPjDVIpDYeU8qA9GAvlzY1uyz-ScQnwzqal6MLyMMC-2JwAApreiH5A5AAyoTw4VvZhhOm_bj2IzPZPyk5byM6nWffnKEbA?width=1202&height=902&cropmode=none){:height="70%" width="70%"}

최대 힙은 데이터로 구성된 이진 트리의 가장 큰 값이 최상위 노드에 위치하는 것을 말한다.

### 최대 힙에서의 삽입, 삭제 과정

#### 삽입

![최대 힙 삽입](https://jvy1fw.bn.files.1drv.com/y4myRvlevijBrGy6p5nYRlstCnpKMvMB_JnnqO7WR7GErEoVGv-Xdu3S1TJvBLq9gBh9e2IavJz6mVBtL0QszEm8uLWRE5RKhOMq5zRHimcvxRLk8BXENsbAVaA562zW2W75s9rWvwMU8Byqfy9CzLVRhtspo6H_js0bmHU56I2apA3bvCslRYDiwi2P6_S0WE7ooKLZ-kCEtc_J2yqzJRVlQ?width=2296&height=582&cropmode=none)

1. 기존 힙에 22를 추가한 경우, 일단 22를 힙의 다음 위치(10의 밑)에 추가한다.
2. 22가 10보다 크므로 둘의 위치를 바꾼다.
3. 22가 19보다 크므로 둘의 위치를 바꾸고, 19는 10보다 크므로 현재 위치를 확정한다.

#### 삭제

![최대 힙 삭제](https://esfm9w.bn.files.1drv.com/y4mKeVytPLuoXVX94hP4zL1qQUrBCQ0inpesDYyeXLLF36ga-UEBUQ-sTqnx8DhEDf2q9C8Jx_ICvyge-6JLsnub8GgERqTcLEyyjAMb4BqIJaJdHlwJwb_oW61K8JN1kZQUxrdU4qVih-VemDOq2qbHtZvfiTOcS35i3PBJlqdjmue7tUNXW5qMj3FyEQpmElRSs__ZOD5h78PwlHq3JD1qQ?width=2298&height=1302&cropmode=none)

삭제는 가장 우선순위가 높은 것이 빠져나간 후에 힙을 새로 구성하는 절차이다.

1. 24를 삭제한다고 가정하자.
2. 맨 마지막 노드인 7을 24 자리에 올린다.
3. 19가 7보다 크므로 둘의 위치를 바꾼다.
4. 17이 7보다 크므로 둘의 위치를 바꾼다.
5. 9가 7보다 크므로 둘의 위치를 바꾼다.

### 최대 힙 구현(Javascript)

![최대 힙 구현](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d2/Heap-as-array.svg/1206px-Heap-as-array.svg.png)

* 힙은 배열로 구현한다.
* 특정 노드의 배열 인덱스가 `current`라고 한다면, 부모 노드는 `current/2`를 통해 찾아갈 수 있고, 자식 노드는 `current*2(좌측 자식 노드)` 또는 `current*2+1(우측 자식 노드)`을 통해서 찾아갈 수 있다.
* 구현을 쉽게 하기 위해 배열을 사용할 때 인덱스를 1부터 사용할 때도 있다.

| 메서드 | 설명 |
| :--- | :--- |
| insert(data) | 힙에 데이터 추가 |
| bubbleUp() | 힙 재정렬. 데이터 삽입 시 호출되는 메서드. Percolate-up, sift-up, trickle-up, swim-up, heapify-up 또는 cascade-up이라고도 한다. |
| extractMax() | 가장 높은 값(루트)을 제거하고 힙을 재정렬한다.  Bubble-down, percolate-down, sift-down, down-heap, trickle down, heapify-down, cascade-down나 extract-min/max라고도 한다. |
| sinkDown(current) | 힙 재정렬. 데이터 삭제 시 호출되는 메서드. |

```js
class BinaryHeap {
  constructor() {
    this.heap = [30, 20, 10, 7, 9, 5]
  }

  // 힙에 요소 추가
  insert(data) {
    this.heap.push(data)
    this.bubbleUp();
  }

  // 일반 배열을 힙으로 구성
  bubbleUp() {
    let index = this.heap.length - 1;

    while (index > 0) {
      let element = this.heap[index];
      let parentIndex = Math.floor((index - 1) / 2);
      let parent = this.heap[parentIndex];

      // 부모가 요소보다 크거나 같을 경우 루프 중단
      if (parent >= element) break;

      // 부모와 요소 교환
      this.heap[index] = parent;
      this.heap[parentIndex] = element;
      index = parentIndex;
    }
  }

  // 루트 노드를 추출하고 정렬
  extractMax() {
    const max = this.heap[0];
    // 힙의 첫번째 요소를 마지막 요소로 초기화
    this.heap[0] = this.heap.pop();
    this.sinkDown(0);
    return max
  }

  sinkDown(current) {
    let left = 2 * current + 1;
    let right = 2 * current + 2;
    let largest = current;
    const length = this.heap.length;

    // 최상단 루트의 값과 왼쪽, 오른쪽 자식의 값과 비교를 해 큰값을 largest 변수에 넣는다.
    if (left <= length && this.heap[left] > this.heap[largest]) {
      largest = left
    }
    if (right <= length && this.heap[right] > this.heap[largest]) {
      largest = right
    }

    // 가장 큰 값의 인덱스가 현재 인덱스와 같지 않을 경우
    if (largest !== current) {
      // 가장 큰 값의 인덱스와 현재 인덱스의 값을 바꿔 준다.
      [this.heap[largest], this.heap[current]] = [this.heap[current], this.heap[largest]]
      // 최대 힙이 다 구성되지 않은 상태이니, 구성할 때까지 재귀적 호출을 한다.
      this.sinkDown(largest)
    }
  }

}

const heap = new BinaryHeap();
heap.insert(90);
heap.insert(50);
// console.log(heap); //BinaryHeap { heap: [ 90, 50, 30, 20, 9, 5, 10, 7 ] }
heap.extractMax();
console.log(heap); //BinaryHeap { heap: [ 50, 30, 20, 9, 5, 10, 7 ] }
```

## References

* 그림으로 정리한 알고리즘과 자료구조
* [위키피디아 - 트리 순회](https://ko.wikipedia.org/wiki/%ED%8A%B8%EB%A6%AC_%EC%88%9C%ED%9A%8C)
* [How to implement Heap Data structure in JavaScript](https://reactgo.com/javascript-heap-datastructure/)