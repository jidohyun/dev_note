---

---
---

`promise`는 비동기 작업을 좀 더 편리하게 처리 할 수 있도록 ES6에 도입된 기능이다 이전에는 비동기 작업을 처리 할 때는 [[콜백함수]]로 처리를 해야 했는데 비동기 작업이 많아질 수록 콜백지옥이 나타나서 가독성이 매우 떨어짐

한번 숫자 n을 파라미터로 받아와서 다섯번에 걸쳐 1초마다 1씩 더해서 출력하는 작업을 setTimeout 으로 구현해보자.

```js
function increaseAndPrint(n, callback) {
  setTimeout(() => {
    const increased = n + 1;
    console.log(increased);
    if (callback) {
      callback(increased);
    }
  }, 1000);
}

increaseAndPrint(0, n => {
  increaseAndPrint(n, n => {
    increaseAndPrint(n, n => {
      increaseAndPrint(n, n => {
        increaseAndPrint(n, n => {
          console.log('끝!');
        });
      });
    });
  });
});
```

이런 식의 코드를 콜백지옥이라고 한다.

![](https://i.imgur.com/oaSookY.png)

비동기적으로 처리해야 하는 일이 많아질수록, 코드의 깊이가 계속 깊어지는 걸 방지 할 수 있다.

### Promise 만들기

Promise는 다음과 같이 만든다.

```js
const myPromise = new Promise((resolve, reject) => {
  // 구현..
})
```

Promise는 성공 할 수도 있고, 실패 할 수도 있다. 성공 할 때에는 resolve를 호출해주면 되고, 실패할 때에는 reject를 호출해 주면 된다. 일단 1초 뒤에 성공시키는 것만 구현해보면,

```js
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve(1);
  }, 1000);
});

myPromise.then(n => {
  console.log(n);
});
```

resolve 를 호출 할 때 특정 값을 파라미터로 넣어주면, 이 값을 작업이 끝나고 나서 사용할 수 있다. 작업이 끝나고 나서 또 다른 작업을 할 때에는 Promise 뒤에 `.then(...)` 을 붙여서 사용하면 된다.

이번엔, 1초 뒤에 실패하게 만들어보면,

```js
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject(new Error());
  }, 1000);
});

myPromise
  .then(n => {
    console.log(n);
  })
  .catch(error => {
    console.log(error);
  });
```

실패하는 상황에서는 `reject`를 사용하고, `catch`를 통하여 실패 했을 시 수행 할 작업을 설정 할 수 있다.

이제, Promise 를 만드는 함수를 작성해보면, 

```js
function increaseAndPrint(n) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const value = n + 1;
      if (value === 5) {
        const error = new Error();
        error.name = 'ValueIsFiveError';
        reject(error);
        return;
      }
      console.log(value);
      resolve(value);
    }, 1000);
  });
}

increaseAndPrint(0).then((n) => {
  console.log('result: ', n);
})
```

![](https://i.imgur.com/5LktObI.png)

여기까지만 보면, 결국 함수를 전달하는건데, 뭐가 다르지 싶을 수 있다. 하지만, Promise 의 속성 중에는, 만약 then 내부에 넣은 함수에서 또 Promise 를 리턴하게 된다면, 연달아서 사용 할 수 있다.

```js
function increaseAndPrint(n) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const value = n + 1;
      if (value === 5) {
        const error = new Error();
        error.name = 'ValueIsFiveError';
        reject(error);
        return;
      }
      console.log(value);
      resolve(value);
    }, 1000);
  });
}

increaseAndPrint(0)
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .catch(e => {
    console.error(e);
  });
```

![](https://i.imgur.com/FKfMN6S.png)

위 코드는 이렇게 정리할 수 있다.

```js
function increaseAndPrint(n) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const value = n + 1;
      if (value === 5) {
        const error = new Error();
        error.name = 'ValueIsFiveError';
        reject(error);
        return;
      }
      console.log(value);
      resolve(value);
    }, 1000);
  });
}

increaseAndPrint(0)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .catch(e => {
    console.error(e);
  });
```

Promise 를 사용하면, 비동기 작업의 개수가 많아져도 코드의 깊이가 깊어지지 않게 된다.

하지만, 이것도 불편함이 있어서 에러를 잡을 때 몇 번째에서 발생했는지 알아내기도 어렵고 특정 조건에 따라 분기를 나누는 작업도 어렵고, 특정 값을 공유해가면서 작업을 처리하기도 까다롭다.
