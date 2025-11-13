
### 개요

Chunk 함수는 인자로 배열과 `size`를 받아서 배열 `size`만큼의 조각으로 나눈 후 새로운 배열을 return 하는 함수이다

```ts
ex)
const arr = [1, 2, 3, 4, 5, 6, 7];

console.log(chunk(arr, 2));
// 👉 [[1, 2], [3, 4], [5, 6], [7]]

console.log(chunk(arr, 3));
// 👉 [[1, 2, 3], [4, 5, 6], [7]]

console.log(chunk(arr, 4));
// 👉 [[1, 2, 3, 4], [5, 6, 7]]

```

### 코드

```ts
export function chunk<T>(arr: readonly T[], size: number): T[][] {
	if (!Number.isInteger(size) || size < 1) {
		throw new Error('Size must be an integer greater than zero.');
	}

	const result: T[][] = [];

	for (let i = 0; i < arr.length; i += size) {
		result.push(arr.slice(i, i + size));
	}
	return result;
}
```

함수의 인자로 `arr`와 `size`를 받고 같이 받은 제네릭 타입 T의 2차원 배열을 리턴합니다

### 예외 처리

함수의 라인1 에서 `size`가 정수가 아니거나 1 미만일때 Error를 리턴하여
`size`가 정수가 아니거나 문자열일때를 방지합니다.

`!Number.isInterger(size)`는 `size`가 정수가 아닐때 true를 리턴합니다
```ts
Number.isInteger(10)      // true
Number.isInteger(10.5)    // false
Number.isInteger("10")    // false
Number.isInteger(NaN)     // false
```

### 구현체

```ts
const result: T[][] = [];

for (let i = 0; i < arr.length; i += size) {
	result.push(arr.slice(i, i + size));
}
```

`const result: T[][] = [];`는 chunk함수가 실행되고 인자로 받은 `arr`가 쪼개진 후 chunk함수가 적용될 배열인 result를 선언합니다.