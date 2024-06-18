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

비동기적으로 처리해야 하는 일이 많아질수록, 코드의 깊이가 계속 깊어지는 걸 방지할 수 이