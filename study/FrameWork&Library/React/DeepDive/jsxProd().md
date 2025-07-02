
---

[ReactJSXElement.js:306-376](https://vscode.dev/github/facebook/react/blob/v19.1.0/packages/react/src/jsx/ReactJSXElement.js#L306-L376)
### 전체 구조

```js
export function jsxProd(type, config, maybeKey) {
  ...
  return ReactElement(...);
}
```

- `jsxProd`는 JSX를 JS 코드로 변환할 때 호출되는 함수이다.
- 컴파일 타임에 Babel이나 SWC 등이 아래처럼 바꿔버리기 때문이다.

```jsx
<MyComponent foo="bar" />
```
->
```js
jsxProd(MyComponent, { foo: "bar" })
```

### 1. key 초기화

```js
let key = null;
```

- JSX 엘리먼트의 `key`를 저장할 변수
- `key`는 React가 엘리먼트를 식별할 때 쓰는 고유 ID 같은 개념이다.

### 2. maybeKey 처리

```js
if (maybeKey !== undefined) {
  if (__DEV__) {
    checkKeyStringCoercion(maybeKey);
  }
  key = '' + maybeKey;
}
```

- JSX 문법상 key가 이렇게 들어올 때가 있음

```jsx
<div {...props} key="hi" />
```

- 위처럼 spread props와 key를 동시에 쓰는 상황이 많아서,
	- `maybeKey`가 명시적 key인지 체크 후 
	- 문자열로 변환해서 저장
- `__DEV__`일 땐 경고 체크도 한다.

### 3. config.key 처리

```js
if (hasValidKey(config)) {
  if (__DEV__) {
    checkKeyStringCoercion(config.key);
  }
  key = '' + config.key;
}
```

- `config`가 `{ key: ... }` 를 가진 경우 Key를 추출
- `hasValidKey`:
	- config에 key가 있는지 확인하는 유틸 함수
- 개발 환경에서는 key가 올바른 타입인지 경고를 띄움

### 4. props 준비

이제 최종 props 객체를 결정한다.

### Case 1) key가 없으면 config 그대로 사용

```js
if (!('key' in config)) {
	props = config;
}
```

- `config` 자체가 props가 되는 경우
- JSX transform은 항상 새 객체를 넘겨주므로, 그대로 써도 안전하다고 판단