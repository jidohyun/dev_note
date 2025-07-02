
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
- `key`는 React가 엘리먼트를 식볋