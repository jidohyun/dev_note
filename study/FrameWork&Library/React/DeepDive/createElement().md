
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

