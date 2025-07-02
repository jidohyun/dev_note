Context는 리액트 컴포넌트간의 어떠한 값을 공유할수 있게 해주는 기능이다. 주로 Context는 전역적으로 필요한 값을 다룰 때 사용한다. 

> Context를 단순히 "리액트 컴포넌트에서 Props가 아닌 또 다른 방식으로 컴포넌트간에 값을 전달하는 방법이다" 라고 접근을 하시는 것이 좋습니다.

### Props로만 데이터를 전달하면 발생할 수 있는 문제

리액트 애플리케이션에서는 일반적으로 컴포넌트에게 데이터를 전달해주어야 할 때 Props를 통해 전달한다. 그런데 깊숙히 위치한 컴포넌트에 데이터를 전달해야 하는 경우에는 여러 컴포넌트를 거쳐 연달아서 Props를 설정해주어야 하기 때문에 불편하고 실수할 가능성이 높아진다.

```jsx
function App() {
  return <GrandParent value="Hello World!" />;
}

function GrandParent({ value }) {
  return <Parent value={value} />;
}

function Parent({ value }) {
  return <Child value={value} />;
}

function Child({ value }) {
  return <GrandChild value={value} />;
}

function GrandChild({ value }) {
  return <Message value={value} />;
}

function Message({ value }) {
  return <div>Received: {value}</div>;
}
```

이러한 코드를 Props Drilling 이라고 부른다. 컴포넌트를 한 두개정도 거쳐서 Props를 전달하는거라면 괜찮지만 이렇게 4개정도를 거쳐서 전달하게 된다면, 불편할 것이다.

