
---

### JSX

React를 사용하기 전에는 HTML 파일로 DOM 구조를 그리고, JS파일로 DOM에 동적인 변화를 줬다.

React도 페이지를 표시하기에, DOM을 표현한다. 하지만 우리가 알던 것과는 조금 다른 과정으로 화면을  그린다. 

React에서 우리는 JSX로 앱의 구조를 표현하고, 컴포넌트는 해당 JSX를 return 한다.
반환된 JSX들은 최종적으로 컴포넌트 루트인 `App.js`에 모이고, `App.js`가 반환한 `<App />`은 `index.js`에서 렌더링 된다.

React 공식문서는 JSX는 `createElement()` 메소드를 거쳐서, `ReactElement`로 변환된다고 한다.
`ReactElement`는 화면에 그리고 싶은 것을 