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

- 개발 모드(DEV) -> 디버깅 정보를 최대한 넣음
- 프로덕션 모드 -> 최소한 정보만 남김

### 개발 모드

### 1. element 객체 생성

```js
element = {
  $$typeof: REACT_ELEMENT_TYPE,
  type,
  key,
  props,
  _owner: owner,
};
```

- `$$typeof`
    - Symbol(react.element)
    - React가 “이 객체는 Element”라고 인식하기 위한 플래그
- `type`
    - div, span, 혹은 컴포넌트 함수
- `key`
    - 리스트 렌더링 시 엘리먼트 구분하는 키
- `props`
    - JSX의 모든 속성이 들어감
- `_owner`
    - 이 엘리먼트를 생성한 컴포넌트를 가리킴
    - 디버깅에 사용

### 2. ref 처리 (개발 모드)

ref가 null이 아닌 경우:
```js
Object.defineProperty(element, 'ref', {
  enumerable: false,
  get: elementRefGetterWithDeprecationWarning,
});
```

- ref에 접근하면 경고 메시지 띄우도록 getter를 설정
- 앞으로는 element.ref를 직접 쓰지 말라는 의미
- `enumerable: false` -> 테스트 비교 시 ref가 안 보이게 숨김

ref가 null인 경우:
```js
Object.defineProperty(element, 'ref', {
  enumerable: false,
  value: null,
});
```

- ref가 없으면 그냥 null로 고정

### 3. Validation Store

```js
element._store = {};
Object.defineProperty(element._store, 'validated', {
  configurable: false,
  enumerable: false,
  writable: true,
  value: 0,
});
```

- 개발 모드에서만 존재
- React가 내부적으로 "이 엘리먼트 validate 되었나?" 체크할 때 씀
- 예) key warning 중복 방지

### 4. Debug 정보들


```js
Object.defineProperty(element, '_debugInfo', {
  configurable: false,
  enumerable: false,
  writable: true,
  value: null,
});
Object.defineProperty(element, '_debugStack', {
  configurable: false,
  enumerable: false,
  writable: true,
  value: debugStack,
});
Object.defineProperty(element, '_debugTask', {
  configurable: false,
  enumerable: false,
  writable: true,
  value: debugTask,
});
```

- 서버 컴포넌트 디버깅용으로 쓰임
- `_debugStack`
	- "이 엘리먼트를 누가 렌더했나?" -> 스택 트레이스
- `_debugTask`
	- 비동기 task 디버깅용

### 5. Object.freeze

```js
if (Object.freeze) {
  Object.freeze(element.props);
  Object.freeze(element);
}
```

### 프로덕션 모드

```js
element = {
  $$typeof: REACT_ELEMENT_TYPE,
  type,
  key,
  ref,
  props,
};
```

- `_owner` 없음
- 디버그 정보 없음
- freeze 없음
- -> 가벼운 객체를 생성해 렌더링 속도 향상

### 마지막 return

```js
return element;
```

- 만들어진 React Element 객체를 반환
- 이 객체는 React Fiber가 사용하여 Virtual DOM Tree를 만든다.

### 예시

아래 JSX:
```jsx
<div className="foo" ref={myRef} />
```

JS로 변환되면:

```js
ReactElement(
  "div",
  null,
  undefined,
  undefined,
  owner,
  { className: "foo", ref: myRef },
  debugStack,
  debugTask
);
```

-> ReactElement 반환값 예시 (dev 모드):

```js
{
  $$typeof: Symbol(react.element),
  type: "div",
  key: null,
  props: { className: "foo" },
  ref: [getter with warning],
  _owner: ...,
  _store: { validated: 0 },
  _debugStack: ...,
  _debugTask: ...
}
```

### 정리

- **ReactElement**는 Virtual DOM의 최소 단위
- dev 모드 -> 디버깅 정보, freeze
- prod 모드 -> 최소 정보만
- `$$typeof`로 React가 이 객체가 "React Element"인지 판별
- ref 접근 시 deprecation warning 띄움
