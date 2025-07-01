
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

### 3. jsxDEV 변수

```js
const jsxDEV: any = __DEV__ ? _jsxDEV : undefined;
```

- 개발 모드 전용 함수
- Babe의 개발 빌드에서만 사용됨
- 추가 디버깅 정보 (예: 파일명, 라인번호, col 번호) 포함

예: 
```jsx
<MyComp prop="value" />
```

-> dev transform 시 아래처럼 변환될 수 있음:

```js
jsxDEV(MyComp, {
  prop: "value"
}, undefined, false, __source, __self);
```

- `__source` -> 파일명, 라인, 컬럼
- `__self` -> 현재 컴포넌트 `this`

즉 디버깅에만 필요한 API이다. 

### 4. Fragment export 

```js
export { REACT_FRAGMENT_TYPE as Fragment, jsx, jsxs, jsxDEV };
```

- JSX에서 `<></>` 사용할 때 필요한 Fragment를 export
- `REACT_FRAGMENT_TYPE` -> Symbol 값이 들어 있음

예: 
```jsx
<>
  <span />
  <div />
</>
```

-> transform 후:

```js
jsxs(Fragment, {
  children: [...]
});
```

### 종합 요약

- `jsx` → children 1개일 때 호출됨
- `jsxs` → children 여러 개일 때 호출됨
- `jsxDEV` → 개발 모드에서 디버깅용으로 호출됨
- `Fragment` → `<></>` 같은 fragment element 지원