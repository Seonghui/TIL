---
layout: default
title: 스택
parent: 자료구조
grand_parent: algorithm
nav_order: 2
---

# 스택
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

![스택](https://4otg3w.bn.files.1drv.com/y4mMUvn_RbpwYG_R3ESJXbLr2Sz0wctSPUz-9vNZf-1ZU8WShS_6hrijPOf0uTxYnWFEc6sN47ccooO2CS6G4K5dGboxP1TM9Q7IGtMu0SMtBVnQiSMsS9cxO9HiTHi5E_ROygwindvOFaFTbvsZCbrzEhXZD4Q5bNCnCygVUK-itZpUT4hOCT-Oau8r_R7UfDhh77XR6O8tInYBad5oAg7Xw?width=1582&height=402&cropmode=none)

데이터가 입력되면 입력되는 순서대로 쌓고, 나중에 들어온 것부터 먼저 사용하는 자료구조이다. 스택 형태의 데이터 구조를 LIFO(Last In First Out)형이라고 하며, 스택에 데이터를 넣는 것을 **PUSH**, 데이터를 꺼내는 것을 **POP**이라고 한다. 스택의 구현은 배열을 이용해도 되고, 연결 리스트를 이용해도 된다.

```js
function Stack() {
  Stack = [];
  Stack.push('A');
  Stack.push('B');
  Stack.push('C');

  console.log(Stack);

  while (Stack.length) {
    console.log(Stack.pop());
  }
}

Stack(); // C B A
```

## 관련 메서드

연결 리스트에서 insert 메서드를 삭제하고 pop, peek, isEmpty 메서드를 추가 구현하면 된다.

| 메서드 | 설명 |
| :--- | :--- |
| append(data) | 노드 추가 |
| find(data) | 노드 찾기 |
| display() | 스택의 모든 요소 출력 |
| findPrevious(data) | 이전 노드 찾기 |
| remove(data) | 노드 삭제 |
| pop() | 마지막 노드 제거 |
| peek() | 마지막 노드 반환 |
| isEmpty() | 스택이 비어있는지 확인 |

## 연결 리스트로 스택 구현(Javascript)

```js
function Node(data) {
  this.data = data;
  this.next = null;
}

function Stack() {
  this.head = null;
}

// 노드 추가
Stack.prototype.append = function (data) {
  var node = new Node(data);
  var current;

  // 리스트가 비어있을 경우
  if (this.isEmpty()) {
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

// 노드 찾기
Stack.prototype.find = function (data) {
  var current = this.head;
  while (current.data !== data) {
    current = current.next;
  }
  return current;
}

// 리스트의 요소 출력
Stack.prototype.display = function () {
  if (this.isEmpty()) {
    console.log('리스트가 비어있습니다.');
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

// 이전 노드 찾기
Stack.prototype.findPrevious = function (data) {
  var current = this.head;
  while (current.next.data !== data) {
    current = current.next;
  }
  return current;
}

// 노드 삭제
Stack.prototype.remove = function (data) {
  // 첫 번쩨 노드일 경우
  if (this.head.data === data) {
    this.head = this.head.next;
  } else { // 첫 번째가 아닐 경우
    var prev = this.findPrevious(data)
    prev.next = prev.next.next;
  }
}

// 가장 마지막에 들어간 노드를 제거
Stack.prototype.pop = function () {
  var node = this.peek();
  // 만약 리스트의 노드가 하나밖에는 없다면
  if (this.head.next === null) {
    this.head = null;
  } else {
    var prev = this.findPrevious(node.data);
    prev.next = null;
  }
}

// 마지막 노드 반환
Stack.prototype.peek = function () {
  var current = this.head;
  while (current.next !== null) {
    current = current.next;
  }
  return current;
}

// 스택이 비어있는지 확인
Stack.prototype.isEmpty = function () {
  return this.head === null;
}

var list = new Stack();
list.append('A');
list.append('B');
list.append('C');
list.display(); // ABC
list.pop();
list.display(); // AB
list.pop();
list.append('D');
list.pop();
list.display(); // A
```

## 스택의 응용 - 후위 표기법

### 후위 표기법

역폴란드 표기법(RPN, reverse Polish notation) 또는 후위 표기법(후치 표기법)(後位 -, postfix notation)은 연산자를 연산 대상의 뒤에 쓰는 연산 표기법이다.

예를 들어, 중위 표기법에서 "1 + 2"와 같은 의미를 지니는 식은 역폴란드 표기법으로는

```text
1 2 +
```

가 된다. 또한, (2 + 3) * 4를 역폴란드 표기법으로 쓰면 다음과 같다.

```text
2 3 + 4 *
```

이와 같은 방식은 수식을 계산할 때 특별한 변환이 필요없이, **수식을 앞에서부터 읽어 나가면서 스택에 저장하면 된다**는 장점이 있다. 또한, 중위 표기법에서는 연산자의 우선순위가 모호해서 괄호가 필요한 경우가 있지만, 역폴란드 표기법에서는 그러한 문제점이 발생하지 않는다. 그러나 인간의 눈으로 쉽게 이해하거나 계산하기 힘들다는 문제점이 있어 눈에 보이는 표기보다는 주로 프로그램 내부의 표기법으로 사용한다.

#### 중위 표기법을 후위 표기법으로 변환하는 과정

다음은 K = ((A * B)-(C / D)) 식을 스택을 이용해서 처리하는 과정이다.

![스택](https://esfj9w.bn.files.1drv.com/y4mD-qwqQCciQNG5CossR2VZ74BX0SWWEFXBqTK7Ju1Ot7x8dJMmKGoJ0DGyiOHQr-EG94lKarNUL1FlIFtIJCp6x9pGVFTD4UQMKfP40gjX_6cBCHiTum5eFWhXNOn8xNt3uzwnYTEgPAmhw9d3jjcJcK_cy2HhJl1qvnN3356wJzoGNkl-M8qryhKgKjd75_oM1DfK0_zkSjMW7mgjThCnw?width=902&height=244&cropmode=none){:height="70%" width="70%"}

초기 상태.

![스택](https://rjukmg.bn.files.1drv.com/y4mAT15c_NHAL3zbzGxBq-RxJWqF8nOhE3Hip6ksGhVR0pzTerGYNgmWyeP-zQaG1uz66QnZrYqUt90qqhmSsyfoTWseQBbG6tcz2YI4QRk33eyRwXiNNSGviLBm8TQeNsaB09vqnsFOySA6hcgrmRdMsKi7q1fcmjWpg3q_RXilvjadurNo5deoRlePMsUn1aeekI0u1GEHO46WrcE-M-OXw?width=902&height=244&cropmode=none){:height="70%" width="70%"}

((A * B)-(C / D))에서 처음 괄호 2개 '(('를 무시하고 'A'를 출력한다.

![스택](https://rxaypq.bn.files.1drv.com/y4mSz_Dv7lw_lWbi2hqeuX5ChC09jIx1WeJ0kdeFS6ZLfgw238yqtMvqlsH1ej7ESWSeRhInuv0cg3yQJUQ1O8fOIFBFNrM4cFPpfBQMocHrhIty624CocwMcfMAjsw0s786Ipy2y0RHWMGD7mhlXe5sn2seJOpTaWMS4th9X5mfIDYmR-P-mXMvHCZujJJz8A9a5U_glRWEwZnKlVtJDoOeg?width=902&height=244&cropmode=none){:height="70%" width="70%"}

((A * B)-(C / D))에서 *을 스택에 PUSH하고 'B'를 출력한다.

![스택](https://4otz3w.bn.files.1drv.com/y4mmK1qpFnyDCFo8kITXhMXmPu-MONy9aiJBcZWVOmMJFNtTRcuoNvzKrVFVr29kvyWKYLmfysgvIQXND6j6FnaReGr2ANEq89iwDFSk5joL2YwBwGrCyic18oHQTZgkGnwcYIkVk1zz5WGgRrj88ggYJgDx1bpaSZrUsCsb3691DOcQw5rOvWyY_KbBNO-7tLitWCP_oh46JMkOoyolfT91Q?width=902&height=244&cropmode=none){:height="70%" width="70%"}

((A * B)-(C / D))에서 ')'를 만나게 되면 스택에 있던 '*'를 출력으로 이동한다.

![스택](https://ffmagg.bn.files.1drv.com/y4mEQgQc1hQArkcvojANn7v4GzdcK5cILF6z-naZVVw6pAQsQjaDqpEdoTMlLKbDj2yke337hZk40rPQ_OSLeydADWkSfiADMvJRxIh-dDAjvQA4VESQsp_g0Vdw5n2he7hVDoJHqNTtmmaP67qtn4ce6UTlbmXIh2qC81xRl7eIlACdpsJDV29Q51MInFtOK9fFNSweAYG1EF7LD6QOFicwQ?width=902&height=244&cropmode=none){:height="70%" width="70%"}

((A * B)-(C / D))에서 '-'를 만나면 스택으로 이동한다. '('를 만나면 무시하고 'C'를 만나면 출력한다.

![스택](https://gg4ojq.bn.files.1drv.com/y4mc4Bh_3YVhFHqZcoc0o3Mlh_jl_tYA3C3-aOocPOWXcyiwrpSWxM9USqW6SpT-jRh9E1L2oS7zfm2E5dYNedRP0MbE_Fde_v8KHrsyvqEirPI8MB_B7v3Ruif80bEfGgVzvCN3N8iGhbjvhf6pe7wSVDie-XjCnVXz7-zOqHxd4LDaYEjZ9Oby15nBi_YZMqb13CJxdceymVPbvrbEaPCDA?width=902&height=244&cropmode=none){:height="70%" width="70%"}

((A * B)-(C / D))에서 '/'를 만나면 스택으로 이동한다. 'D'를 만나면 출력한다.

![스택](https://splpxw.bn.files.1drv.com/y4m1zbtMyjp6CqdF3vC_IVDfSzf3sjzcWtSzkP2Pci0Cji-dM_vULDQLjCTyRscR7Tl4tXDT3AaqWgfb4NF3tClL6RdC6fSwy7lD21ulEQojfx13LHTPrmeSBzvCA9QBhVX3RybSgqCRVQ_ZzRLT5zjv_RGB34gx7MA_3RubnOagxxxePYXSZE3XNicP8RjFPj83yg2Kt4b5lVXp52mR8ChKg?width=902&height=244&cropmode=none){:height="70%" width="70%"}

((A * B)-(C / D))에서 ')'을 만나면 스택의 값('/')을 출력으로 이동한다. 또, ')'을 만나면 스택의 값('*')을 출력으로 이동한다.

#### 후위 표기법 표기 수식의 연산 과정

![스택](https://i4cs3q.bn.files.1drv.com/y4mJyQEgzdfdurzZNL0R5NYAlFNaY2114i3tWPBizsV6_xNnUqAt0cM_BbqQ80Blwad712_mdIO9E3Y49mZSSchen0P0h_NdbYQhhrlD5c2R7isVIgaa7IXnYLEEs3eCB-rXazzku2Ctc38zKguS3W_Gtyrhlm3vhxeyzVuw3OUus3nWo9BbDvIETRi3kdTDSZVAhWs4_p3yce2njkCHqFRpw?width=562&height=242&cropmode=none){:height="50%" width="50%"}

AB*CD/-에서 A를 스택에 PUSH하고, B를 스택에 PUSH한다.

![스택](https://tld3ag.bn.files.1drv.com/y4mxCgL_HZuZPDZgDntwRFF060SP-BnkQHWCueoFDCS03ExR3CfpHEU_E_GGeMDfHoHIBLQ8X1EV-vuooShTQYDAFAZbbWhmRkLogh66oluriz-zg0D3D_--ZAUFQSGhgRUcaIU_ZsG8aOYl3SQ7oxI52s4I8DKNJMziOuCOgFsUU2GPv3ZT0m0O7aOl7220b7KdLFHw_KpZSjuN-TqFDagKA?width=562&height=242&cropmode=none){:height="50%" width="50%"}

AB*CD/-에서 *를 만나면 B를 스택에서 POP하고, A를 스택에서 POP한 다음에 * 연산한 결과(X)를 다시 스택에 넣는다.

![스택](https://jvy5fw.bn.files.1drv.com/y4m_FgtHrXkEASvGjbdCpEPolkel-DkktKul-n7vVoeXn5THZP3BEFR6a0EiBLFGaw_Uh16Rc3slF4dkG0f-scPVMokyDIpnmwVJca2yn7rNL2Xaz8rmitzbYPQK1tGF_1QEKF6n1rtLrhiJ_eGYv59PN9h_fulfJFCZGivIWS1CVV0VZqKwcTRDSpIGVftoD9uzfh8koAsEW5QoZMi1M9QxA?width=562&height=302&cropmode=none){:height="50%" width="50%"}

AB*CD/-에서 C, D를 만나면 이 값을 스택에 PUSH한다.

![스택](https://esfq9w.bn.files.1drv.com/y4mZjnsG-WM2IIGADrledmZzbEVkGi3pisoV3eS2iD9qWnJruM-TIsk1A0yNnOZlwWw7W0eH9SaR1deh4YBHfLE7dBUYLEq5eeOfqT55zSo2-vqrxMSHk6A-XxC8fWXy7qWRETHqW1_fmrjlwtbfNmBswLEwCaI7rQbuZt8ZTJsTDo-LRBDLEa8Jmcw9LRuqfX6syUIavKG_wmL51Tnql30Bw?width=562&height=242&cropmode=none){:height="50%" width="50%"}

AB*CD/-에서 /를 만나면 C,D를 POP한 후에 C/D 연산을 수행한다. 그리고 결과(Y)를 스택에 넣는다.

![스택](https://rjurmg.bn.files.1drv.com/y4mdWSkic5uHfJY_k-32cFRX1djky7CGtjflV0y3DnD6_wDam3up1dOsKIBEw8lmn8ZNek4mrp9W7nnxw1iVHCiWxxL0CJmKnqn5SZQPab2mtjs6pwF9gLZrPSqnAYNp_sQeKYLZNgjHRqxcxGGBqQeSgzdMNH8aiPSeGlwiO4fzLtzmoftSaRSqQXk1HeLGQ3H2j_3kAdGl9EtxPE4Kl9Ptg?width=562&height=242&cropmode=none){:height="50%" width="50%"}

AB*CD/-에서 -를 만나면 X, Y를 POP한 후에 X-Y 연산을 수행한다. 그리고 결과(Z)를 스택에 넣는다.

## References

* 그림으로 정리한 알고리즘과 자료구조
* [위키피디아 - 역폴란드 표기법](https://ko.wikipedia.org/wiki/%EC%97%AD%ED%8F%B4%EB%9E%80%EB%93%9C_%ED%91%9C%EA%B8%B0%EB%B2%95)
