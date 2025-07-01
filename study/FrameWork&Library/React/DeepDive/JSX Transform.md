
[ReactJSX.js:17-28](https://vscode.dev/github/facebook/react/blob/v19.1.0/packages/react/src/jsx/ReactJSX.js#L17-L29)

### 1. jsx 변수

```js
const jsx: any = __DEV__
  ? jsxProdSignatureRunningInDevWithDynamicChildren
  : jsxProd;
```

- `jsx(...)` 는 **React.createElement**를 대체하는 함수
- JSX 문법을 컴파일하면 jsx 호출로 변환됨
- children이 1개일 때 사용됨 