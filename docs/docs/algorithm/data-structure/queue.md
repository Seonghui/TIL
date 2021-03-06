---
layout: default
title: 큐
parent: 자료구조
grand_parent: algorithm
nav_order: 3
---

# 큐
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

![큐](https://tld5ag.bn.files.1drv.com/y4m7IJl2jCE-1A4L59S5XGd2OOObEtov165IzWlgPAIGHKMKERcQzIfLZyHSl9qRxEQS6BtZK7GgIDkXtB3ShPIobNe-cE8nLshFcTINkqnZwqX6s4WSI8IF84hxxx1_OXjND4TNtr1o0C9Ai48VnpJqXnLGxUgWLNVr76Hac5JMJ9rOCvwCTVZhF3DKzfzy2671BVyt_nKa-ICjvQostkaDQ?width=822&height=252&cropmode=none){:height="70%" width="70%"}

데이터가 입력되면 입력되는 순서대로 쌓고, 먼저 들어온 것부터 먼저 사용하는 자료구조이다. 데이터가 들어오는 위치는 가장 뒤(Rear 또는 Back이라고 한다.)에 있고, 데이터가 나가는 위치는 가장 앞(Front라고 한다.)에 있다. 큐 형태의 데이터 구조를 FIFO(First In First Out)형이라고 한다. 큐의 구현은 배열을 이용(순환 큐)하거나, 또는 연결 리스트를 이용(링크드 큐)한다.

큐 자료구조는 많이 사용되므로 많은 프로그래밍 언어에서 기본적으로 제공된다. 자바스크립트는 아래 두 메서드로 손쉽게 큐를 구현할 수 있다.

* push(): 배열의 끝부분에 데이터를 추가
* shift(): 배열의 앞부분에서 데이터를 삭제

```js
function Queue() {
  Queue = [];
  Queue.push('A');
  Queue.push('B');
  Queue.push('C');
  Queue.push('D');

  console.log(Queue);

  while (Queue.length) {
    console.log(Queue.shift());
  }
}

Queue(); // A B C D
```

## 관련 메서드

| 메서드 | 설명 |
| :--- | :--- |
| enqueue(data) | 큐에 노드를 추가 |
| dequeue() | 큐에서 노드를 삭제 |
| clear() | 큐의 모든 노드 삭제 |
| find(data) | 노드 찾기 |
| front() | 큐의 앞부분에 저장된 노드 반환 |
| back() | 큐의 끝부분에 저장된 노드 반환 |
| isEmpty() | 큐가 비어있는지 확인 |
| display() | 큐의 노드 출력 |

## 큐 구현

### 연결리스트로 큐 구현 (Javascript)

```js
function Node(data) {
  this.next = null;
  this.data = data;
}

function Queue() {
  this.head = null;
}

// 큐에 요소를 추가
Queue.prototype.enqueue = function (data) {
  var node = new Node(data);
  var current;
  if (this.isEmpty()) {
    this.head = node;
  } else {
    current = this.head;
    while (current.next !== null) {
      current = current.next;
    }
    current.next = node;
  }
}

// 큐에서 요소를 삭제
Queue.prototype.dequeue = function () {
  if (this.isEmpty()) {
    console.log('큐가 비어있습니다.');
  } else if (this.head.next === null) {
    console.log(this.head.data)
    this.head = null;
  } else {
    console.log(this.head.data);
    this.head = this.head.next;
  }
}

// 큐가 비어있는지 확인
Queue.prototype.isEmpty = function () {
  return this.head === null;
}

// 큐의 앞부분에 저장된 요소 반환
Queue.prototype.front = function () {
  console.log(this.head.data);
}

// 큐의 끝부분에 저장된 요소 반환
Queue.prototype.back = function () {
  var current = this.head;
  while (current.next !== null) {
    current = current.next;
  }
  console.log(current.data);
}

// 큐의 모든 요소 삭제
Queue.prototype.clear = function () {
  return this.head = null;
}

// 큐의 요소 출력
Queue.prototype.display = function () {
  if (this.isEmpty()) {
    console.log('큐가 비어있습니다.');
  } else {
    var result = [];
    var current = this.head;
    while (current !== null) {
      result.push(current.data);
      current = current.next;
    }
    result = result.join(',');
    console.log(result);
  }
}

var queue = new Queue();
queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);
queue.enqueue(4);
queue.enqueue(5);
queue.display(); // 1,2,3,4,5
queue.dequeue(); // 1
queue.dequeue(); // 2
queue.dequeue(); // 3
queue.display(); // 4,5
queue.enqueue(1);
queue.display(); // 4,5,1
queue.front(); // 4
queue.back(); // 1
queue.clear();
queue.display(); // 큐가 비어있습니다.
```

### 배열로 큐 구현(Javascript)

```js
function Queue() {
  this.list = []
}

Queue.prototype.enqueue = function (data) {
  this.list.push(data);
}

Queue.prototype.dequeue = function () {
  this.list.shift();
}

Queue.prototype.isEmpty = function () {
  return this.list.length === 0;
}

Queue.prototype.front = function () {
  console.log(this.list[0]);
}

Queue.prototype.back = function () {
  console.log(this.list[this.list.length - 1]);
}

Queue.prototype.clear = function () {
  this.list = [];
}

Queue.prototype.display = function () {
  if (this.isEmpty()) {
    console.log('큐가 비어있습니다.');
  } else {
    console.log(this.list);
  }
}

var queue = new Queue();

queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);
queue.enqueue(4);
queue.enqueue(5);
queue.display(); // 1,2,3,4,5
queue.dequeue();
queue.dequeue();
queue.dequeue();
queue.display(); // 4,5
queue.enqueue(1);
queue.display(); // 4,5,1
queue.front(); // 4
queue.back(); // 1
queue.clear();
queue.display(); // 큐가 비어있습니다.
```

### 스택 2개로 큐 구현 (Javascript)

stack2에 아무것도 없는 상태에서 dequeue를 수행하려고 할 때, stack1에 쌓여있는 데이터들을 전부 stack2로 옮기는 것이 포인트이다.

![스택 2개로 큐 구현]({{site.url}}/TIL/assets/images/algorithm/queue/stack-queue-1.png)

```js
class Stack {
  constructor() {
    this.stackArray = [];
  }
  push(item) {
    this.stackArray.push(item);
  }
  pop() {
    return this.stackArray.pop();
  }
  peek() {
    return this.stackArray[this.stackArray.length - 1]
  }
  isEmpty() {
    return this.stackArray.length === 0;
  }
  display() {
    console.log(this.stackArray)
  }
}

class Queue {
  constructor() {
    this.stack1 = new Stack();
    this.stack2 = new Stack();
  }
  enqueue(item) {
    this.stack1.push(item);
  }
  dequeue() {
    // 스택 2가 비어있으면
    if (this.stack2.isEmpty()) {
      // 스택 1의 아이템이 비워질때까지
      while (!this.stack1.isEmpty()) {
        // 스택 1의 아이템을 스택 2로 옮긴다
        this.stack2.push(this.stack1.pop());
      }
    }
    // 스택 2의 아이템을 dequeue한다.
    return this.stack2.pop();
  }
}

const q = new Queue();
q.enqueue(1);
q.enqueue(2);
q.enqueue(3);
q.enqueue(4);
console.log(q.dequeue());//1
console.log(q.dequeue());//2
q.enqueue(5);
q.enqueue(6);
console.log(q.dequeue());//3
console.log(q.dequeue());//4
console.log(q.dequeue());//5
console.log(q.dequeue());//6
```

## 우선순위 큐(Priority Queue)

우선순위 큐의 요소들은 우선순위를 가지고 있다. 일반 큐는 선형 자료구조이지만, 우선순위 큐는 들어간 순서에 상관없이 우선순위가 높은 데이터가 먼저 나온다. 우선순위 큐가 힙이라는 것은 널리 알려진 오류이다. 우선순위 큐는 리스트나 맵과 같이 추상적인 개념이다. 리스트는 연결 리스트나 배열로 구현될 수 있는 것과 같이, 우선순위 큐는 힙이나 다양한 다른 방법을 이용해 구현될 수 있다.

하지만 배열의 경우에는 데이터 삽입 및 삭제 과정에서 데이터를 한 칸씩 당기거나 밀어야 하는 연산을 계속해야 하고, 삽입의 위치를 찾기 위해 배열에 저장된 모든 데이터와 우선순위를 비교해야 한다. 연결리스트는 삽입의 위치를 찾기 위해 첫 번째 노드에서부터 시작해 마지막 노드에 저장된 데이터와 우선순위 비교를 진행할지도 모른다. 그래서 우선순위 큐는 주로 힙을 이용해 구현하는 것이 일반적이다. 구현에 대해서는 [힙](/TIL/docs/algorithm/data-structure/tree.html#힙-heap)을 참고.

우선순위 큐는 최소한 다음 연산이 지원되어야 한다.

* 우선순위를 지정하여 큐에 추가한다.
* 가장 높은 우선순위를 가진 원소를 큐에서 제거하고 이를 반환한다.

## 원형 큐(Circular Queue)

선형 큐의 문제점(배열로 큐를 선언할시 큐의 삭제와 생성이 계속 일어났을때, 마지막 배열에 도달후 실제로는 데이터공간이 남아있지만 오버플로우가 발생)을 보완한 것이 원형 큐이다. front가 큐의 끝에 닿으면 큐의 맨 앞으로 자료를 보내어 원형으로 연결 하는 방식이다. 환형 큐라고도 한다.

| ENQ(A) | ENQ(B) | ENQ(C) | ENQ(D) | DEQ(A) | ENQ(E) |
| :--- | :--- | :--- | :--- | :--- | :--- |
|  |  |  | D | D | D |
|  |  | C | C | C | C |
|  | B | B | B | B | B |
| A | A | A | A |  | E |

```js
class CircleQueue {
  constructor(size) {
    this.maxSize = size;
    this.array = [];
    this.front = 0;
    this.rear = 0;
  }

  // front와 rear가 같은 위치에 있다면 큐가 비어있다는 뜻
  isEmpty() {
    return this.front == this.rear;
  }

  // rear가 front의 바로 전 위치에 있다면 큐가 가득 찼다는 뜻
  isFull() {
    return (this.rear + 1) % this.maxSize == this.front;
  }

  // 데이터가 들어갈 때는 rear만 움직인다
  // rear의 위치는 최근에 들어온 데이터의 위치이다
  // 즉 새로운 데이터가 들어오기 위해 먼저 이동해야 한다
  enQueue(item) {
    if (this.isFull()) {
      console.log(new Error('큐가 포화상태입니다.'))
    } else {
      // rear를 이동시키고
      this.rear = (this.rear + 1) % this.maxSize;
      // 그 자리에 데이터를 넣는다.
      this.array[this.rear] = item;
    }
  }

  deQueue() {
    if (this.isEmpty()) {
      console.log(new Error('큐가 비었습니다.'));
    } else {
      // 데이터가 나갈 때는 front만 움직인다.
      this.front = (this.front + 1) % this.maxSize;
      return this.array[this.front];
    }
  }

  print() {
    if (this.isEmpty()) {
      console.log(new Error('큐가 비었습니다.'));
    }
    let string = '';
    let i = this.front;
    do {
      i = (i + 1) % this.maxSize;
      string += this.array[i] + ' ';
      if (i == this.rear) {
        console.log(string);
        break;
      }
    } while (i != this.front);
  }
}

let queue = new CircleQueue(5);

queue.enQueue(1);
queue.enQueue(2);
queue.enQueue(3);
queue.enQueue(4);
queue.deQueue();
queue.enQueue(5);
queue.print();
```

## References

* 그림으로 정리한 알고리즘과 자료구조
* [위키백과 - 큐](https://ko.wikipedia.org/wiki/%ED%81%90_(%EC%9E%90%EB%A3%8C_%EA%B5%AC%EC%A1%B0))
* [위키백과 - 우선순위 큐](https://ko.wikipedia.org/wiki/%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84_%ED%81%90)
* [원형 큐 (Circular Queue) 자료 구조](https://lktprogrammer.tistory.com/59)
