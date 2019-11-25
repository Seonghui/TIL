---
layout: default
title: async and await
parent: 8. 비동기적 프로그래밍
grand_parent: javascript
nav_order: 3
---

## 1. async and await

우선 함수 앞에 async를 붙여주고 비동기로 처리되는 곳에 await를 추가한다. 여기서 주의할 점은 await 뒷부분은 반드시 promise를 반환해야 하며, async함수 자체도 promise를 반환한다. async await은 거의 동기 코드랑 비슷하게 짤 수 있게 해준다.

async는 프로미스를 반환하니까 명시적으로 프로미스 코드를 안 써줘도 된다. await은 promise를 기다린다.

```js
function doubleAfter2Seconds(x) {
  console.log('value:' + x)
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x * 2);
    }, 2000);
  });
}
async function addAsync(x) {
  // await은 Promise를 기다린다.
  const a = await doubleAfter2Seconds(10); // 20
  const b = await doubleAfter2Seconds(20); // 40
  const c = await doubleAfter2Seconds(30); // 60
  
  // Promise 리턴
  // 10 + 20 + 40 + 60
  return x + a + b + c;
}

// promise를 return하기 때문에 then으로 받을 수 있다.
addAsync(10).then((sum) => {
  console.log('result:' + sum);
});

// Console Output:
// "value:10"
// "value:20"
// "value:30"
// "result:130"
```

async 함수가 호출되면 Promise를 리턴한다. async함수에서 값을 리턴하면, promise는 그 값을 받아서 resolved된다. async함수는 await 표현식을 포함하고 있으며 async 함수에 Promise 값이 전달되기 전까지 실행을 지연시킨다.



## 2. async and await 예외처리

`async` 와 `await`은` try` `catch`로 예외를 처리할 수 있다.

```js
function doubleAfter2Seconds(x) {
  console.log('value:' + x)
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x * 2);
    }, 2000);
  });
}
async function addAsync(x) {
  try {
    // await은 Promise를 기다린다.
    const a = await doubleAfter2Seconds(10); // 20
    const b = await doubleAfter2Seconds(20); // 40
    const c = await doubleAfter2Seconds(30); // 60

    // Promise 리턴
    // 10 + 20 + 40 + 60
    return x + a + b + c;
  } catch (error) {
    console.log(error)
  }
}

// promise를 return하기 때문에 then으로 받을 수 있다.
addAsync(10).then((sum) => {
  console.log('result:' + sum);
});

// Console Output:
// "value:10"
// "value:20"
// "value:30"
// "result:130"
```



## References

* [자바스크립트 비동기 프로그래밍 promise, async, await](https://medium.com/@shlee1353/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%B9%84%EB%8F%99%EA%B8%B0-async-await-promise-ae659eb1cb7e)
* [[JS] async/await으로 콜백지옥을 해결해보자](https://victorydntmd.tistory.com/87)
