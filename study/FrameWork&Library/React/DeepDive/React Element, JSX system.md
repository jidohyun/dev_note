
### 개요

React의 핵심 API들은 `packages/react/index.js:27-77`에서 export 되며, 이는 React Element 생성과 JSX 변환을 담당하는 핵심 시스템이다.

[packages/react/index.js:27-77](https://vscode.dev/github/facebook/react/blob/v19.1.0/packages/react/index.js#L27-L76)

### 핵심 Element 생성 API

React Element 생성의 핵심은 `createElement`와 JSX 런타임 함수들이다.
[[createElement()]] 함수는 `ReactJSXElement.js:638-708`에서 구현되며, 전통적인 React Element 생성 방식을 제공한다.

[ReactJSXElement.js:638-708](https://vscode.dev/github/facebook/react/blob/v19.1.0/packages/react/src/jsx/ReactJSXElement.js#L638-L764)

JSX 런타임은 현대적인 JSX 변환을 위해 `ReactJSX.js:17-28` 에서 정의된다. [[JSX Transform]]

[ReactJSX.js:17-28](https://vscode.dev/github/facebook/react/blob/v19.1.0/packages/react/src/jsx/ReactJSX.js#L17-L29)

### React Element 구조

React Element의 실제 구조는 `ReactJSXElement.js:176-254`의 [[ReactElement()]] 함수에서 정의된다.

[ReactJSXElement.js:176-254](https://vscode.dev/github/facebook/react/blob/v19.1.0/packages/react/src/jsx/ReactJSXElement.js#L176-L298)

핵심 속성들:

- `$$typeof`: React Element임을 식별하는 심볼
- `type`: 컴포넌트 타입 (함수, 클래스, 또는 문자열)
- `key`: 리스트 렌더링에서 사용되는 고유 식별자
- `props`: 컴포넌트에 전달되는 속성들
- `_owner`: 개발 모드에서 디버깅을 위한 소유자 정보

### JSX 변환 시스템

프로덕션 JSX 변환은 `ReactJSXElement.js:306-376` 의 [[jsxProd()]] 함수에서 처리된다.

[ReactJSXElement.js:306-376](https://vscode.dev/github/facebook/react/blob/v19.1.0/packages/react/src/jsx/ReactJSXElement.js#L306-L376)

개발 모드 JSX 변환은 `ReactJSXElement.js:460-557` 의 `jsxDEV` 함수에서 추가적인 검증과 디버깅 정보를 포함한다.