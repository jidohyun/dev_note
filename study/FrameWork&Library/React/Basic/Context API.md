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