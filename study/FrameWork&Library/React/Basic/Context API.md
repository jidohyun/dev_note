[다른 사람들이 안 알려주는 리액트에서 Context API 잘 쓰는 방법](https://velog.io/@velopert/react-context-tutorial)
### 1. Context API란?

- **목적:** React 컴포넌트 트리 안에서 **데이터를 `prop` 드릴링 없이 전역적으로 공유**하기 위한 방법이다.
    
- **핵심:** 부모 컴포넌트가 자식 컴포넌트에게 `prop`을 계속해서 전달(prop drilling)할 필요 없이, 원하는 컴포넌트에서 Context에 접근하여 데이터를 읽거나 업데이트할 수 있게 한다.
    
- **주요 사용처:** 테마(다크 모드/라이트 모드), 사용자 인증 정보, 지역화(언어 설정) 등 애플리케이션 전반에 걸쳐 필요한 전역 데이터 관리에 사용한다.
    

---

### 2. Context API의 핵심 구성 요소

Context API는 크게 `Context` 생성, `Provider`, `Consumer` (혹은 `useContext` Hook)의 세 가지 요소로 구성된다.

#### 2.1. `React.createContext()`: Context 생성

- **역할:** 새로운 Context 객체를 생성한다.
- **인자:** Context의 **기본값(default value)**을 받는다. 이 기본값은 `Provider`가 없을 때 `Consumer`가 참조하게 되는 값이다.
- **코드 예시:**

```jsx
import React from 'react';

// Context 생성, 기본값은 'dark'
const ThemeContext = React.createContext('dark');

export default ThemeContext;
```

#### 2.2. `Context.Provider`: Context 값 제공

- **역할:** Context를 구독하는 하위 컴포넌트들에게 **Context 값을 제공**한다.
- **사용법:** `Provider` 컴포넌트로 값을 공유하고자 하는 자식 컴포넌트들을 감싼다. `value` prop을 통해 실제 공유할 데이터를 전달한다.
- **특징:** `Provider`의 `value` prop이 변경되면, 해당 `Provider`의 모든 하위 `Consumer`들은 리렌더링된다.
- **코드 예시:**

```jsx
import React from 'react';
import ThemeContext from './ThemeContext';
import Toolbar from './Toolbar'; // 하위 컴포넌트

function App() {
  return (
    // ThemeContext의 값을 'light'로 제공
    <ThemeContext.Provider value="light">
      <Toolbar />
    </ThemeContext.Provider>
  );
}
```

#### 2.3. `Context.Consumer`: Context 값 구독 (클래스 컴포넌트에서 주로 사용)

- **역할:** `Provider`가 제공하는 Context 값을 구독하여 사용한다.
- **사용법:** **렌더 프롭(render prop) 패턴**을 사용한다. `Consumer` 컴포넌트의 자식으로 함수를 전달하고, 이 함수의 인자로 Context 값을 받는다.
- **코드 예시:**

```jsx
import React from 'react';
import ThemeContext from './ThemeContext';

// Function Component for example
function ThemedButton() {
  return (
    <ThemeContext.Consumer>
      {theme => ( // theme은 Context.Provider에서 전달된 'value'
        <button style={{ background: theme === 'dark' ? '#333' : '#eee', color: theme === 'dark' ? 'white' : 'black' }}>
          Context Button ({theme})
        </button>
      )}
    </ThemeContext.Consumer>
  );
}

export default ThemedButton;
```

#### 2.4. `useContext()` Hook: Context 값 구독 (함수형 컴포넌트에서 주로 사용)

- **역할:** 함수형 컴포넌트에서 Context 값을 더 간결하게 구독할 수 있도록 돕는다.
- **사용법:** `useContext(Context객체)` 형태로 사용하며, Context 객체를 인자로 전달하면 해당 Context의 현재 값을 반환한다.
- **코드 예시:**

```jsx
import React, { useContext } from 'react';
import ThemeContext from './ThemeContext';

function ThemedButtonWithHook() {
  const theme = useContext(ThemeContext); // Context 값 직접 가져오기

  return (
    <button style={{ background: theme === 'dark' ? '#333' : '#eee', color: theme === 'dark' ? 'white' : 'black' }}>
      Context Button (Hook: {theme})
    </button>
  );
}

export default ThemedButtonWithHook;
```

### 3. Context API 사용 시 주의사항

- **리렌더링 성능:** `Provider`의 `value` prop이 변경될 때마다 하위의 모든 `Consumer` 컴포넌트들이 리렌더링된다. 불필요한 리렌더링을 방지하기 위해 `value` prop에 객체를 전달할 경우 메모리 참조가 변경되지 않도록 `useMemo` 등을 고려해야 한다.
- **대규모 상태 관리에는 부적합:** Context API는 전역 상태 관리를 위한 훌륭한 도구지만, Redux나 Recoil과 같은 전문 상태 관리 라이브러리처럼 복잡한 로직(비동기 처리, 액션 디스패치 등)을 처리하는 데는 한계가 있다. 단순하고 자주 변경되지 않는 전역 데이터에 적합하다.
- **재사용성 고려:** Context를 남용하면 컴포넌트의 재사용성이 떨어질 수 있으므로, 꼭 필요한 경우에만 신중하게 사용하는 것이 좋다.

### 4. 정리

React Context API는 `prop` 드릴링 문제를 해결하고 컴포넌트 트리 내에서 전역 데이터를 효율적으로 공유할 수 있는 강력한 기능이다. `Provider`로 값을 제공하고 `Consumer` 또는 `useContext` Hook으로 값을 구독하는 방식으로 동작한다. 하지만 성능과 확장성을 고려하여 적절한 사용 사례에서 활용하는 것이 중요하다.

