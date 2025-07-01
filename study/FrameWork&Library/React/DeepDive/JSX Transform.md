
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

예:
```jsx
<MyComp prop="value" />
```

이건 [[Babel]] 같은 컴파일러가 아래처럼 바꿔버림:

```js
jsx(MyComp, { prop: "value" });
```

- `__DEV__`가 true → 개발 모드
    - `jsxProdSignatureRunningInDevWithDynamicChildren` (개발용 함수, 디버깅 정보 넣음)
- `__DEV__`가 false → 프로덕션 모드
    - `jsxProd` (최적화된 prod 코드)

### 2. jsxs 변수

```js
const jsxs: any = __DEV__
  ? jsxProdSignatureRunningInDevWithStaticChildren
  : jsxProd;
```

- children이 2개 이상일 때 사용
- 즉 "static children" 다룰 때
- 개발 모드 vs 프로덕션 모드 선택 로직은 똑같음

예:
```jsx
<MyComp>
  <span />
  <div />
</MyComp>
```

-> 아래처럼 변환됨:

```js
jsxs(MyComp, {
  children: [
    jsx("span", {}),
    jsx("div", {})
  ]
});
```

왜 구분하냐?
- children이 배열인지 단일 요소인지에 따라 내부 최적화가 달라질 수 있기 때문

