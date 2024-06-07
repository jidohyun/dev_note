
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
