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

![이진 트리의 분류](https://4otc3w.bn.files.1drv.com/y4mPLgkqi2iy3qE3gsCCO3WXXhsdhnTIdceAzANC1eGGsus7N4kn1RbPJ3Ok4UFln0lGNAg3G98BEdm2rouAoeDP0lAm-hQqGph6uOnduE6w8GuY5YwWfq3PZXeetxp5zs0bobfiixQLGk_gpE757syEJDUXGc0pepMHaMJj8Y6MWyZzv5gICV-bSTK16oKz2biNEVdjtMg5N4qo78Vj2ZGvw?width=1678&height=510&cropmode=none)

이진 트리를 구성하다 보면 여러 가지 모양이 나올 수 있으며, 모양에 따라 3가지로 분류할 수 있다.

* 포화 이진 트리(Full Binary Tree): 레벨의 노드가 꽉 차 있는 트리
* 완전 이진 트리(Complete Binary Tree): 마지막 레벨 전까지는 노드가 꽉 차 있고, 마지막 레벨의 왼쪽에서 오른쪽으로 노드가 채워져 있는 트리
* 편향 이진 트리(Skewed Binary Tree): 왼쪽 혹은 오른쪽 서브 트리만을 가지는 트리

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

## References

* 그림으로 정리한 알고리즘과 자료구조
