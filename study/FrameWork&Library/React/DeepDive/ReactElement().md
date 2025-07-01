[ReactJSXElement.js:176-254](https://vscode.dev/github/facebook/react/blob/v19.1.0/packages/react/src/jsx/ReactJSXElement.js#L176-L298)

ReactElement 함수는 **React Element** 객체를 만드는 핵심 함수다.
React에서 JSX를 쓰면 결국 이 함수를 통해 객체가 생성된다.

### 전체 역할

이 함수는 아래처럼 생긴 객체를 리턴한다:

```js
{
  $$typeof: Symbol(react.element),
  type,
  key,
  ref,
  props,
  _owner,       // 개발 모드에서만
  ...
}
```

이 객체가 **React Element**이다.
-> **Virtual DOM**의 최소 단위이다.

### 1. ref 추출

```js
const refProp = props.ref;
const ref = refProp !== undefined ? refProp : null;
```

- `ref`는 DOM이나 컴포넌트 인스턴스에 접근하기 위한 값
- JSX에서 `<div ref={myRef} />` 과 같음
- `undefined`면 null로 처리한다 (과거 코드 호환)

### 2. 개발 모드 / 프로덕션 분기

```js
let element;
if (__DEV__) {
  ...
} else {
  ...
}
```

