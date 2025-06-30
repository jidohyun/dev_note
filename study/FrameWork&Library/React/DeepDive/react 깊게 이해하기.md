
---

### JSX

React를 사용하기 전에는 HTML 파일로 DOM 구조를 그리고, JS파일로 DOM에 동적인 변화를 줬다.

React도 페이지를 표시하기에, DOM을 표현한다. 하지만 우리가 알던 것과는 조금 다른 과정으로 화면을  그린다. 

React에서 우리는 JSX로 앱의 구조를 표현하고, 컴포넌트는 해당 JSX를 return 한다.
반환된 JSX들은 최종적으로 컴포넌트 루트인 `App.js`에 모이고, `App.js`가 반환한 `<App />`은 `index.js`에서 렌더링 된다.

React 공식문서는 JSX는 `createElement()` 메소드를 거쳐서, `ReactElement`로 변환된다고 한다.
`ReactElement`는 화면에 그리고 싶은 것을 나타내고 있는 <b>객체</b> 이며 이 객체를 통해서 최종적으로 DOM을 그리게 될 것이다.

![](https://i.imgur.com/Vd6k2Zv.png)

*jsx의 변환 과정*

React가 ReactElement를 어떻게 DOM으로 그려내는지 알아내려는 것은 아니다.
`ReactElement`라는 **객체**가 우리가 그릴 UI정보를 담고 있고, 추후에 이 객체들을 활용하게 될 것이라는 것만 우선 인지하면 된다.

---

### 컴포넌트 업데이트 

UI를 갱신할 때, React에서는 DOM api를 사용해 DOM을 바로 수정하는 것이 아닌, state와 하위 컴포넌트에게 넘겨줄 props값을 바꿔주면 UI가 알아서 갱신된다.

여기서 중요한 점은 앱 전체를 새로 갱신하는 것이 아닌, <font color="#92d050">변경된 부분만 업데이트한다</font>는 점이다.
```jsx
function App() {
  const [count, setCount] = useState(0);

  return (
    <div className="App">
      <div className='wrapper'>
        <h2>Simple Counter</h2>
        count: {count}
        <div>
          <button onClick={() => setCount((prev) => prev + 1)}>+1</button>
          <button onClick={() => setCount((prev) => prev - 1)}>-1</button>
        </div>
      </div>
    </div>
  );
}
```

![](https://i.imgur.com/0m6bZe3.png)

컴포넌트의 업데이트는 DOM(UI)에서 필요한 부분만 바꾸게 되는데, 어떻게 이 과정이 진행되는지 알아보자.

---
### Render Phase & Commit Phase

React는 크게 Render phase와 commit phase라는 두 가지 주기를 갖는다.

- Render Phase: **컴포넌트를 렌더링 하고, 필요한 변화를 계산하는 작업.**
- Commit Phase: 해당 변화를 DOM에 적용시키는 과정

Render Phase의 작업 이후 Commit Phase 작업이 수행되기에, Render Phase에 ReactElement를 모아 계산한 값을 넘길 것이다.
그 결과물은 Commit Phase때 실제 DOM에 적용될 것이고,
Render Phase의 **필요한 변화**라는 말은 아마 위 예제에서 `{count}` 부분에 해당하는 위치를 찾아내는 과정일 것이다.

그러면 그 앞의 **렌더링**이라는 표현은 뭘까?
알아보기 전에 꼭 알아야 하는 것은 DOM을 바꾸는 것은 렌더링이 아니다.

---

### 렌더링

> 렌더링은 모든 컴포넌트에게 현재 state와 props값의 조합을 기반으로 각각의 UI를 어떻게 화면에 띄우고 싶어 하는지 물어보는 React의 프로세스입니다. - Mark Erikson (Redux Maintainer)

렌더링은 각각의 **컴포넌트마다**, 자기 자신을 **어떻게 표시하고 싶은지**, 렌더링에 해당되는 **모든 컴포넌트를 돌며 확인하는 과정**이라고 한다.  
여기서 "어떻게 띄워지고 싶은지"에 대해서는 ReactElement로 설명할 수 있다.

모든 컴포넌트의 반환 값인 JSX를 모았다면, 이는 React.createElement() 메소드를 통해, ReactElement 객체가 될 것이다.

ReactElement 객체는 위에서 보았듯, 태그명에 해당하는 `type`, 어트리뷰트에 해당하는 `props`, 태그 사이에 있는 값인 `children` 등, 이 컴포넌트가 어떻게 표시되어야 하는지에 대한 모든 정보가 포함되어있다.

![](https://i.imgur.com/TVizOpY.png)

그런데, 매 렌더링마다 모든 컴포넌트를 돌며 ReactElement들을 수집하는 것은 상식적으로 맞지 않는다.  
당연하게도 React가 렌더링할 컴포넌트는 정해져 있다.

---

### 렌더링 조건

**아래 규칙에 해당되면, 해당되는 컴포넌트들은 렌더링된다.** (함수형 컴포넌트 기준)

>
>   1. useState hook의 두 번째 인자인 setState() 로 인해 state가 변경되었을 때 해당 컴포넌트는 렌더링된다.
>   2. useReducer의 dispatch함수가 실행되어 action으로 state가 변경되었을 때, 해당 컴포넌트는 렌더링 된다.
>   3. 부모 컴포넌트가 렌더링될 때, 모든 자식 컴포넌트는 렌더링 된다.
>   4. "key" 라는 특수한 props값이 변결될 때, 해당 컴포넌트는 새로 렌더링 된다.
>

결론적으로, **state가 변경된 컴포넌트와, 그 컴포넌트의 모든 자식 컴포넌트는 렌더링된다.**

또, 렌더링에서 말하는 "render"는 컴포넌트마다 return하는 JSX를 element로 변환해 받아야 하는 과정이고,
그렇기에 렌더링은 해당 **함수 컴포넌트를 호출하는 과정**이다.

>
>   어떻게 보면 당연하다. 컴포넌트가 return하는 JSX를 받아서 확인하는 과정인데, 컴포넌트가 return하는 값을 받으려면 컴포넌트를, 해당 함수로 호출해야 하지 않을까? App()을 호출하면 App이 return하는 JSX를 가져올 것이다. JSX문법으로는 태그처럼 쓰긴 한다.
>

그럼 렌더링을 확인하려면? 해당 함수 컴포넌트 중간에 `console.log()`를 찍어보면 된다.

함수가 호출되었으니 함수의 맨 윗줄부터 내려가며 실행될 것이고, 컴포넌트가 렌더링이 된다면 필히 `console.log()`가 찍힐 것이다. 

```jsx
function App() {
  const [count, setCount] = useState(0);
  console.log("App Rendered!!")

  return (
    <div className="App">
      // 카운터
    </div>
  );
}
```

![](https://i.imgur.com/btDMDG2.png)

### React Developer tools

렌더링 동작을 더 쉽게 확인하는 방법이 있다.
Chrome 확장 프로그램 중 React Developer tools를 설치하고, 앱을 실행한 뒤 개발자 도구를 열면 몇 가지 탭이 새로 생긴다.

이 확장 프로그램은 앱이 렌더링 되는 것을 시각화 해서 컴포넌트를 파악하기 쉽게 도와주는 프로그램이다.

이 프로그램으로 내가 만든 카운터를 확인해보니까 카운트가 증가 할 때 마다 부모 전체가 렌더링 되는 것을 알았다

*App 렌더링은 최초 한 번만*

state가 App에 있었다면 계속해서 렌더링 하니까 state를 하위 컴포넌트로 옮겨서, state가 필요한 부분만 갱신해야 한다.

> 
>   1. 버튼이 눌려 setCount()를 통해 컴포넌트의 state가 변경됨
>   2. Counter 컴포넌트와 자식 컴포넌트들이 전부 재랜더링 되어야 하나, 자식이 없으므로 혼자 렌더링 됨.
>   3. App의 렌더링 과정에서 Counter 함수를 호출하지만, App과는 관련이 없으므로 App은 렌더링 되지 않음.
>   4. React는 Counter가 반환하는 새로운 ReactElement를 가지고 나중에 Commit Phase로 넘어갈 것임
>

---

### 정리

React는 **컴포넌트가 반환하는 JSX를 모아 변경될 부분을 계산하는 Render Phase와 해당 변화를 실제 DOM에 적용시키는 Commit Phase 두 가지 라이프사이클을 갖는다.**

변경 가능성이 있는 컴포넌트를 먼저 모으는 과정인 렌더링은, **state의 변화가 발생한 컴포넌트와 모든 자식 컴포넌트들을 대상으로 이루어진다.** 여기서 렌더링은 DOM을 변화시키는 과정이 아니다.
컴포넌트가 표현할 UI를, JSX를, ReactElement를 확인하는 과정이다.

>  
>   위 Counter 예시에서 DOM은 {count} 부분만 바뀌었지만, 렌더링은 앱 전체를 대상으로, 다음엔
>   Counter함수를 대상으로 진행되었다. 두 방식 다, DOM의 변화는 {count} 부분에서만 일어났다.
>   

