
### 개요

React의 핵심 API들은 `packages/react/index.js:27-77`에서 export 되며, 이는 React Element 생성과 JSX 변환을 담당하는 핵심 시스템이다.

###### packages/react/index.js:27-77
![](https://i.imgur.com/pVardIN.png)

### 핵심 Element 생성 API

React Element 생성의 핵심은 `createElement`와 JSX 런타임 함수들이다.
createElement 함수는 `ReactJSXElement.js:638-708`에서 구현되며, 전통적인 React Element 생성 방식을 제공한다.


JSX 런타임은 현대적인 JSX 변환을 위해 `ReactJSX.js:17-28` 에서 정의된다.

