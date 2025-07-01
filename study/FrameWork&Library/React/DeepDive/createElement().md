
[ReactJSXElement.js:638-708](https://vscode.dev/github/facebook/react/blob/v19.1.0/packages/react/src/jsx/ReactJSXElement.js#L638-L764)

`createElement`함수는 React 에서 JSX를 JavaScript 코드로 변환할 때 사용하는 핵심 함수로, 최종적으로 React Element 객체를 생성한다.

```jsx
<MyComponent title="Hello" />
```

[[Babel]]은 이를 다음처럼 변환한다.

```js
React.createElement(MyComponent, { title: 'Helllo' })
```

### 함수가 하는 일

`createElement(type, config, ...children)`은 다음과 같은 과정을 거쳐 **React Element 객체**를 반환한다.

### 0. 함수 시그니처

```js
export function createElement(type, config, children) {
```

- `type`: 태그 혹은 컴포넌트 (e.g, `'div'`, `MyComponent`)
- `config`: props 객체
- `children`: JSX children
- 추가로 `argument`에 `children`이 여러 개 올 수 있음

### 1. 개발 모드에서 children key 검사

```js
if (__DEV__) {
	for (let i = 2; i < arguments.length; i++) {
		validateChildKeys(arguments[i], type);
	}
}
```

- 개발 모드라면, children에 **key가 잘 붙어 있는지 검사**
- `<ui>{[<li />, <li />]}</ul>` 같은 리스트 children이 key가 없으면 경고

### 2. 변수 선언

```js
let propName;
const props = {};
let key = null;
```

- `propName`: props 순회용
- `props`: 최종적으로 쓸 props 객체
- `key`: 리엑트 key값 저장

### 3. config 처리

```js
if (config != null) {
```

- config가 null이 아니면 props 추출 시작

### 3-1. 개발 모드: 구버전 JSX transform 경고

```js
if (__DEV__) {
  if (
    !didWarnAboutOldJSXRuntime &&
    '__self' in config &&
    !('key' in config)
  ) {
    didWarnAboutOldJSXRuntime = true;
    console.warn(
      'Your app (or one of its dependencies) is using an outdated JSX ' +
        'transform. Update to the modern JSX transform for ' +
        'faster performance: https://react.dev/link/new-jsx-transform',
    );
  }
}
```

- babel 7.9 이전 JSX transform을 쓸 때 뜨는 경고
- `__self`라는 속성이 구버전 transform의 흔적임

### 3-2. key 추출

```js
if (hasValidKey(config)) {
	if (__DEV__) {
		checkKeyStringCoercion(config.key);
	}
	key = '' + config.key;
}
```

- `config.key`가 있으면 key로 저장
- 문자열로 변환(`'' + config.key`)
- 개발 모드에선 key를 문자열로 강제 변환 시 경고 체크

### 3-3. props 추출

```js
for (propName in config) {
  if (
    hasOwnProperty.call(config, propName) &&
    propName !== 'key' &&
    propName !== '__self' &&
    propName !== '__source'
  ) {
    props[propName] = config[propName];
  }
}
```

- key, __self, __source 제외하고 props 객체에 복사

예)

```js
createElement('div', { id: 'foo', key: 'mykey' })
// props = { id: 'foo' }
```

### 4. children 처리

```js
const childrenLength = arguments.length - 2;
if (childrenLength === 1) {
  props.children = children;
} else if (childrenLength > 1) {
  const childArray = Array(childrenLength);
  for (let i = 0; i < childrenLength; i++) {
    childArray[i] = arguments[i + 2];
  }
  if (__DEV__) {
    if (Object.freeze) {
      Object.freeze(childArray);
    }
  }
  props.children = childArray;
}
```

- 전달된 children 수 파악
- 1개면 그대로 props.children에 넣음
- 2개 이상이면 배열로 만들어 prop.children에 넣음
- 개발 모드에선 Object.freeze로 children 배열을 얼려서 불변성 체크

### 5. defaultProps 처리

```js
if (type && type.defaultProps) {
  const defaultProps = type.defaultProps;
  for (propName in defaultProps) {
    if (props[propName] === undefined) {
      props[propName] = defaultProps[propName];
    }
  }
}
```

- 컴포넌트에 defaultProps가 있으면
- props에 값이 없는 것만 채워 넣음

#### 예)
```js
// MyButton.js
import React from 'react';

function MyButton(props) {
  // props.text와 props.color를 사용
  // 만약 color prop이 전달되지 않았다면 'blue'가 사용됩니다.
  return (
    <button style={{ backgroundColor: props.color, color: 'white', padding: '10px' }}>
      {props.text}
    </button>
  );
}

// defaultProps 설정
MyButton.defaultProps = {
  color: 'blue' // color prop의 기본값은 'blue'
};

export default MyButton;
```

```js
// App.js
import React from 'react';
import MyButton from './MyButton';

function App() {
  return (
    <div>
      <MyButton text="Click Me" /> {/* color prop을 명시적으로 전달 안 함 */}
    </div>
  );
}

export default App;
```

**이때 코드(`if (type && type.defaultProps) { ... }`)의 동작:**
1. `type`은 `MyButton`이고, `MyButton.defaultProps`가 존재합니다.
2. `defaultProps = { color: 'blue' }`가 됩니다.
3. `for` 루프가 `defaultProps`를 순회합니다. `propName`은 `'color'`가 됩니다.
4. `if (props['color'] === undefined)`를 확인합니다. `MyButton text="Click Me"`에서는 `color` prop이 전달되지 않았으므로 `props['color']`는 `undefined`입니다. 이 조건은 `true`가 됩니다.
5. 따라서 `props['color'] = defaultProps['color'];` 즉, `props.color = 'blue';`가 실행됩니다.
6. 결과적으로 `MyButton` 컴포넌트 내에서는 `props.color`가 `'blue'`로 인식되어 **파란색 버튼**이 렌더링됩니다.

```js
// App.js
import React from 'react';
import MyButton from './MyButton';

function App() {
  return (
    <div>
      <MyButton text="Submit" color="red" /> {/* color prop을 'red'로 전달 */}
    </div>
  );
}

export default App;
```

**이때 코드(`if (type && type.defaultProps) { ... }`)의 동작:**

1. `type`은 `MyButton`이고, `MyButton.defaultProps`가 존재합니다.
2. `defaultProps = { color: 'blue' }`가 됩니다.
3. `for` 루프가 `defaultProps`를 순회합니다. `propName`은 `'color'`가 됩니다.
4. `if (props['color'] === undefined)`를 확인합니다. `MyButton text="Submit" color="red"`에서는 `props['color']`가 `'red'`이므로 `undefined`가 아닙니다. 이 조건은 `false`가 됩니다.
5. `props.color`에 기본값이 할당되지 않고, **명시적으로 전달된 `'red'`가 그대로 유지**됩니다.
6. 결과적으로 `MyButton` 컴포넌트 내에서는 `props.color`가 `'red'`로 인식되어 **빨간색 버튼**이 렌더링됩니다.

### 6. 개발 모드: key getter 추가

```js
if (__DEV__) {
  if (key) {
    const displayName =
      typeof type === 'function'
        ? type.displayName || type.name || 'Unknown'
        : type;
    defineKeyPropWarningGetter(props, displayName);
  }
}
```

- 개발 모드에서 props.key 접근 시 경고하도록 getter 추가
- key는 리엑트에서 내부용이라 일반 props로 쓰지 말라는 경고

### 7. 디버깅용 owner stack 체크

```js
const trackActualOwner =
  __DEV__ &&
  ReactSharedInternals.recentlyCreatedOwnerStacks++ < ownerStackLimit;
```

- 개발 모드일 때만 true
- 디버깅용 렌더링 stack trace 생성 여부 결정

### 8. React Element 생성

```js
return ReactElement(
  type,
  key,
  undefined,
  undefined,
  getOwner(),
  props,
  __DEV__ &&
    (trackActualOwner
      ? Error('react-stack-top-frame')
      : unknownOwnerDebugStack),
  __DEV__ &&
    (trackActualOwner
      ? createTask(getTaskName(type))
      : unknownOwnerDebugTask),
);
```

- 실제로 React Element 객체를 생성
- 반환값 예시:

```js
{
  $$typeof: Symbol(react.element),
  type,
  key,
  ref: null,
  props,
  _owner: ...,
  ...
}
```
