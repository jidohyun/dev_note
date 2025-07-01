
[ReactJSXElement.js:638-708](https://vscode.dev/github/facebook/react/blob/v19.1.0/packages/react/src/jsx/ReactJSXElement.js#L638-L764)

`createElement`함수는 React 에서 JSX를 JavaScript 코드로 변환할 때 사용하는 핵심 함수로, 최종적으로 React Element 객체를 생성한다.

```jsx
<MyComponent title="Hello" />
```

Babel은 이를 다음처럼 변환한다.

```js
React.createElement(MyComponent, { title: 'Helllo' })
```

