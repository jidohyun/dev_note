
---

### Promise의 상위호환?

async/await 문법은 ES8에 해당하는 문법으로, Promise의 상위호환으로 사용 할 수 있다.

기본적인 사용법을 알아보면, 

```js
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function process() {
  console.log('안녕하세요!');
  await sleep(1000); // 1초쉬고
  console.log('반갑습니다!');
}

process();
```

async/await 문법을 사용 할 때에는, 함수를 선언 할 때 함수의 앞부분에 `async` 를 붙여야 함. 그리고
promise의 앞부분에 `await`를 넣어주면 해당 프로미스가 끝날 때 까지 기다렸다가 다음 작업을 수행 할 수 있다

위 코드에선 