---
layout: default
title: 리스트형 자료구조
parent: 자료구조
grand_parent: algorithm
nav_order: 2
---

# 리스트형 자료구조

연속적인 저장의 형태를 가지며, 두 가지가 있다.

* 배열: 크기가 변하지 않는 리스트형 자료구조. 중간에 값을 지워도 빈칸으로 유지된다.
* 리스트: 크기가 변하는 자료구조. 중간의 값을 지우면 뒤의 것이 앞으로 이동한다.

만약 리스트의 원소들 사이에 순서를 가지면 선형 리스트(Linear List)라고 한다.

## 연결 리스트 자료구조

저장하는 각 데이터가 데이터 외에 다음과 이전의 데이터를 가리키는 정보를 가지고 있는 구조이다.

연결 리스트는 삽입과 삭제를 할 때는 전체 데이터의 변동은 없고, 앞, 뒤의 연관된 데이터만 변동하면 된다. 그래서 연결 리스트는 중간에 넣거나, 지우는 과정을 빠르게 수행할 수 있다. 하지만 특정 데이터를 찾는 것은 포인터를 따라 이동해야 하므로 성능이 떨어진다. 그러므로 데이터의 변화가 많은 상황이라면 연결 리스트를 사용하는 것이 현명하다.

### 연결 리스트(Linked List)

![연결 리스트](https://rxa2pq.bn.files.1drv.com/y4mXYyPcWsPsKTeK9s53ESwdd21lkzCswMNOmFodAlUuEVDt4mQ0aZG00h7YUhcSGrGFCYvfsacsTDkAHGo5vnwmLAPMK2gXFr4jsN_Mkx3wqtZEM62n2mLdOgyl8XfZuX6dkESMFtuMUF3_YLV6ZUUINuJZXPDHj3Nj4hBgT0198SgdRDcT6LAoinAt3aEpx6LneyVGaNI9iyLG9XPtOXE4g?width=862&height=142&cropmode=none)

저장된 각 데이터가 **데이터(data) + 다음 데이터의 포인터(next)**로 이루어진 것으로, 한 방향으로만 탐색 가능한 구조이다.

#### 연결 리스트 구현(Javascript)

```js
function Node(data) {
  this.data = data;
  this.next = null;
}

function LinkedList() {
  this.head = null;
}

// 노드 추가
LinkedList.prototype.append = function (data) {
  var node = new Node(data);
  var current;

  // 리스트가 비어있을 경우
  if (this.head == null) {
    this.head = node;
  } else { // 리스트가 비어있지 않을 경우
    current = this.head
    // 리스트의 마지막 노드를 찾는다
    while (current.next !== null) {
      current = current.next;
    }
    // 리스트의 마지막 노드에 삽입할 노드 연결
    current.next = node;
  }
}

// 노드 중간 삽입
LinkedList.prototype.insert = function (newData, data) {
  var node = new Node(newData);
  var current = this.find(data);
  node.next = current.next;
  current.next = node;

}

// 노드 찾기
LinkedList.prototype.find = function (data) {
  var current = this.head;
  while (current.data !== data) {
    current = current.next;
  }
  return current;
}

// 연결 리스트의 요소 출력
LinkedList.prototype.display = function () {
  var current = this.head;
  while (current !== null) {
    console.log(current.data);
    current = current.next;
  }
}

// 이전 노드 찾기
LinkedList.prototype.findPrevious = function (data) {
  var current = this.head;
  while (current.next.data !== data) {
    current = current.next;
  }
  return current;
}

// 노드 삭제
LinkedList.prototype.remove = function (data) {
  // 첫 번쩨 노드일 경우
  if (this.head.data === data) {
    this.head = this.head.next;
  } else { // 첫 번째가 아닐 경우
    var prev = this.findPrevious(data)
    prev.next = prev.next.next;
  }
}

var list = new LinkedList();
list.append('A');
list.append('B');
list.append('D');
list.insert('E', 'B');
list.append('F');
list.insert('G', 'A');
list.remove('F');
list.remove('E');
list.remove('A');
list.display() // GBD
```

### 이중 연결 리스트(Doubly Linked List)

![이중 연결 리스트](https://4otd3w.bn.files.1drv.com/y4mBymJi3vI8J8vOyk2EW5pBdOpqPEsg66gbpD4CLyQW3Tsj5vo8JIjX1Ho5qa20LWTKtAgH86FI1fdh-wisW79V83BP5svwGWpwEwDG7b7STKlaDfPDf9ModxuHBFck-HHAidrokw6FTB9VYDArHR7IL_znhKAI6-8J8F1D1gj4phv1SZUCGihUWIyq52A833XrP-d3crvPN3xm0ATerWS_g?width=1084&height=142&cropmode=none)

더블 원형 리스트라고도 한다. 저장된 각 데이터가 **이전 데이터의 포인터(prev) + 데이터(data) + 다음 데이터의 포인터(next)**로 이루어진 것으로, 양방향 탐색 가능 구조이다.

#### 이중 연결 리스트 구현(Javascript)

```js
function Node(data) {
  this.data = data;
  this.next = null;
  this.prev = null;
}

function DoublyLinkedList() {
  this.head = null;
}

// 노드 추가
DoublyLinkedList.prototype.append = function (data) {
  var node = new Node(data);
  var current;

  // 리스트가 비어있을 경우
  if (this.head == null) {
    this.head = node;
  } else { // 리스트가 비어있지 않을 경우
    current = this.head
    // 리스트의 마지막 노드를 찾는다
    while (current.next !== null) {
      current = current.next;
    }
    // 리스트의 마지막 노드에 삽입할 노드 연결
    current.next = node;
    node.prev = current;
  }
}

// 노드 중간 삽입
DoublyLinkedList.prototype.insert = function (newData, data) {
  var node = new Node(newData);
  var current = this.find(data);
  node.next = current.next;
  current.next = node;
  node.prev = current;
  node.next.prev = node;

}

// 노드 찾기
DoublyLinkedList.prototype.find = function (data) {
  var current = this.head;
  while (current.data !== data) {
    current = current.next;
  }
  return current;
}

// 연결 리스트의 요소 출력
DoublyLinkedList.prototype.display = function () {
  var current = this.head;
  while (current !== null) {
    console.log(current.data);
    current = current.next;
  }
}

// 이전 노드 찾기
DoublyLinkedList.prototype.findPrevious = function (data) {
  var current = this.head;
  while (current.next.data !== data) {
    current = current.next;
  }
  return current;
}

// 노드 삭제
DoublyLinkedList.prototype.remove = function (data) {
  // 첫 번쩨 노드일 경우
  if (this.head.data === data) {
    this.head = this.head.next;
    this.head.prev = null;
  } else { // 첫 번째가 아닐 경우
    var current = this.find(data);
    // 맨 끝 노드일 경우
    if (current.next == null) {
      current.prev.next = null;
      current.prev = null;
    } else {
      current.prev.next = current.next;
      current.next.prev = current.prev;
      current.prev = null;
      current.next = null;
    }
  }
}

var list = new DoublyLinkedList();
list.append('A');
list.append('B');
list.append('D');
list.insert('E', 'B');
list.append('F');
list.insert('G', 'A');
list.remove('F');
list.remove('E');
list.remove('A');
list.display() // GBD
```

### 원형 연결 리스트(Circular Linked List)

![원형 연결 리스트](https://gg4vjq.bn.files.1drv.com/y4m0F7ESv-TglRqrQfzH6QYv6fKe7z-yH7rUKDf06HwKIevQx6KrobVaJ_kpwvrY6uwgjcp3fBQ0KY3XweVNByvfL_-eIF8F7LiOf6V6xzFnFSy-bmH_oCrUEH_XHkZD6Y2bnLyI1npj2u1qbIyTAiXKL1CfKPgc0rtFfBpXy2Q1jel3j9h7rq8snnMMWnjGQKQg7dVjTLYIOZ1Q7xamMX2eg?width=972&height=176&cropmode=none)

환형 연결 리스트라고도 한다. 연결 리스트의 양끝이 연결되어있는 구조이다. 이중 연결 리스트의 양 끝이 연결되어 있으면 이중 원형 연결 리스트가 된다.
원형 연결 리스트는 마지막에 노드를 삽입하는 것이 첫 번째 노드를 삽입하는 것과 같은 의미를 가진다.

#### 원형 연결 리스트(Javascript)

```js
function Node(data) {
  this.data = data;
  this.next = null;
}

function CircularLinkedList() {
  this.head = null;
}

// 노드 추가
CircularLinkedList.prototype.append = function (data) {
  var node = new Node(data);
  var current;

  // 리스트가 비어있을 경우
  if (this.head == null) {
    this.head = node;
    this.head.next = this.head;
  } else { // 리스트가 비어있지 않을 경우
    current = this.head
    // 리스트의 마지막 노드를 찾는다
    while (current.next !== this.head) {
      current = current.next;
    }
    // 리스트의 마지막 노드에 삽입할 노드 연결
    current.next = node;
    // 노드의 next에 head 를 연결시켜준다
    node.next = this.head;
  }
}

// 노드 중간 삽입
CircularLinkedList.prototype.insert = function (newData, data) {
  var node = new Node(newData);
  var current = this.find(data);
  node.next = current.next;
  current.next = node;

}

// 노드 찾기
CircularLinkedList.prototype.find = function (data) {
  var current = this.head;
  while (current.data !== data) {
    current = current.next;
  }
  return current;
}

// 연결 리스트의 요소 출력
CircularLinkedList.prototype.display = function () {
  var current = this.head;
  // 마지막 노드까지 반복
  while (current.next !== this.head) {
    console.log(current.data);
    current = current.next;
  }
  console.log(current.data);
}

// 이전 노드 찾기
CircularLinkedList.prototype.findPrevious = function (data) {
  var current = this.head;
  while (current.next.data !== data) {
    current = current.next;
  }
  return current;
}

// 노드 삭제
CircularLinkedList.prototype.remove = function (data) {
  // 첫 번쩨 노드일 경우
  if (this.head.data === data) {
    var prev = this.findPrevious(this.head.data)
    this.head = this.head.next;
    prev.next = this.head;
  } else { // 첫 번째가 아닐 경우
    var prev = this.findPrevious(data)
    prev.next = prev.next.next;
  }
}

var list = new CircularLinkedList();
list.append('A');
list.append('B');
list.append('D');
list.insert('E', 'B');
list.append('F');
list.insert('G', 'A');
list.remove('F');
list.remove('E');
list.remove('A');
list.display() // GBD
```