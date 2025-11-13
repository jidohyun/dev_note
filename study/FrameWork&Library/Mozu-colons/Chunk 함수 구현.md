
### 개요

Chunk 함수는 인자로 배열과 size를 받아서 배열을 조각으로 나눈 후 return 하는 함수이다

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

### 