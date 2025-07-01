
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

### 1. Key 추출 (config로부터)

- `key`는 React가 리스트를 렌더링할 때 각 요소를 식별하는 데 사용하는 고유값이다.
- `config.key`가 있으면 문자열로 변환하여 따로 분리해 저장한다.


