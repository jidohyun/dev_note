
---

### Context api?

Context api는 리액트의 내장 api이다 
앱에서 컴포넌트에게 props를 사용하지 않고 필요한 데이터를 전송할 수 있다.
따라서 앱의 모든 컴포넌트에서 사용할 수 있는 데이터를 전달할 때 유용하다.

Context api를 쓰면 편할 것 같지만 치명적인 단점이 있다.
바로 Context api를 쓰면 동작이 일어날 때마다 리렌더링이 된다.

그래서 지금은 거의 안쓰는 기술이다.
(다크모드는 씀)


### 상태관리 라이브러리?

우리는 react hook인 useState를 주로 사용하여 상태관리를 한다.
하지만 중요한 변수가 늘어날 수록 useState의 상태관리가 어려워지는데..

이렇게 많은 상태를 관리하기 편하게 해주는 라이브러리가 있는데..
그의 이름은 zustand이다.

### 사용법

```ts
import { create } from 'zustand'
export const use이름Store = create((set, get) => { 
	return { 
		상태: 초깃값,
		액션: 함수 
	} 
})
```

`set`과 `get` 매개변수는 다음과 같이 각 액션에서 사용할 수 있다.

`get`함수를 호출하면, 상태와 액션을 가진 스토어 **객체를 얻을 수 있다**.
또한 `set`을 호출하면, **상태를 변경할 수 있다.**

`set`함수를 호출할 때 콜백을 사용하면, `get`함수를 사용하지 않아도 바로 스토어 객체를 얻을 수 있다.
변경할 상태를 속성으로 포함한 객체를 콜백에서 반환해야 한다.

```tsx
import { create } from 'zustand' 
export const use이름Store = create((set, get) => {
	return {
		상태: 초깃값, 
		액션: () => { 
			const state = get() 
			const { 상태 } = state 
			set({ 상태: 상태 + 1 }) 
		}
	} 
})
```

`set` 함수를 호출할 때 콜백을 사용하면 `get` 함수를 사용하지 않아도 바로 스토어 객체를 얻을 수 있다
변경할 상태를 속성으로 포함한 객체를 콜백에서 반환해야 한다.

```tsx
import { create } from 'zustand'
export const use이름Store = create(set => {
	return {
		상태: 초깃값,
		액션: () => {
			set(state => ({ 상태: state.상태 + 1}))
		}
	}
})
```

컴포넌트에서 스토어 훅을 가져와 호출하면, 상태와 액션을 가진 객체를 얻을 수 있다.
또한 상태는 반응형이기 때문에, 상태가 변경되면 컴포넌트가 다시 렌더링된다.

```tsx
import { use이름Store } from '~/store/스토어'
export default function 컴포넌트() {
	const 상태 = use이름Store(state => state.상태)
	const 액션 = use이름Store(state => state.액션)
	return (
		<>
			<h2>상태</h2>
			<button onClick={액션}>+</button>
		</>
	)
}
```

콜백 없이 스토어 훅을 호출하면 액션이 아닌 스토어 객체를 얻을 수 있지만, 이는 사용하지 않는 상태가 변경되어도 컴포넌트가 다시 렌더링 되기 때문에 대부분은 권장하지 않는다.

```tsx
import { use이름Store } from '~/store/스토어'
export default function 컴포넌트() {
	const 스토어 = use이름Store()
	return (
		<>
			<h2>상태</h2>
			<button onClick={액션}>+</button>
		</>
	)
}
// 권장하지 않음
```

### 예제

프로젝트의 `store` 폴더에서 각 스토어를 생성한다.
타입스크립트를 사용할 때에는 `create` 함수의 제네릭으로 상태와 액션 타입을 전달한다.

```ts
create<타입>()
```

`get` 함수를 호출하면, 상태와 액션이 포함된 스토어 객체를 얻을 수 있다.
이를 통해, 각 액션에서 상태의 값을 얻을 수 있다.

```tsx
import { create } from 'zustand'

export const useCountStore = create<{
	count: number 
	increase: () => void 
	decrease: () => void 
}>((set, get) => ({ 
	count: 1, 
	increase: () => { 
		const { count } = get() 
		set({ count: count + 1 }) 
	}, 
	decrease: () => { 
		const { count } = get() 
		set({ count: count - 1 }) 
	} 
}))
```

`get` 함수를 사용하지 않고 `set`의 콜백을 사용하면 더 간결하게 상태를 변경할 수 있다.

```tsx
import { create } from 'zustand'

export const useCountState = create<{
	count: number
	increase: () => void
	decrease: () => void
}>(set => ({
	count: 1,
	increase: () => set(state => ({ count: state.count + 1})),
	decrease: () => set(state => ({ count: state.count - 1}))
}))
```

생성한 스토어를 사용해보면

```tsx
import { useCountStore } from './store/count' 

export default function App() { 
	const count = useCountStore(state => state.count) 
	const increase = useCountStore(state => state.increase) 
	const decrease = useCountStore(state => state.decrease) 
	return ( 
		<> 
			<h2>{count}</h2> 
			<button onClick={increase}>+1</button> 
			<button onClick={decrease}>-1</button>
		</> 
	)	
}
```

### 액션 분리

만약 여러 컴포넌트에서 단일 스토어의 액션을 많이 사용한다면, 액션을 분리하고 관리하는 패턴을 쓸 수 있다
다음과 같이 액션 객체 안에서 모든 액션을 관리하면 각 컴포넌트에서 필요한 액션만 가져오기 쉽다.

```tsx
import { create } from 'zustand' 

export const useCountStore = create<{
	count: number 
	actions: { 
		increase: () => void 
		decrease: () => void 
	} 
}>(set => ({ 
	count: 1, 
	actions: { 
		increase: () => set(state => ({ count: state.count + 1 })), 
		decrease: () => set(state => ({ count: state.count - 1 })) 
	} 
}))
```

```tsx
import { useCountStore } from './store/count' 

export default function App() { 
	const count = useCountStore(state => state.count) 
	const { increase, decrease } = useCountStore(state => state.actions)
	return ( 
		<> 
			<h2>{count}</h2> 
			<button onClick={increase}>+1</button> 
			<button onClick={decrease}>-1</button>
		</> 
	)	
}
```

### 상태 초기화

만약 상태를 초깃값으로 되돌리는 기능이 필요한 경우 다음과 같이 `resetState` 함수를 추가해서 사용할 수 있다. 액션을 제외한 상태만 초기화하는 것이니, 상태와 액션을 분리해서 타입과 초깃값을 작성한다.

```tsx
import { create } from 'zustand' 

interface State { 
	count: number 
	double: number 
	min: number 
	max: number 
} 

interface Actions { 
	actions: { 
		increase: () => void 
		decrease: () => void 
		resetState: () => void 
	} 
} 

const initialState: State = { 
	count: 1, 
	double: 2, 
	min: 0,
	max: 99
}

export const useCountStore = create<State & Actions>(set => ({ 
	...initialState,
	actions: { 
		increase: () => set(state => ({ count: state.count + 1 })), 
		decrease: () => set(state => ({ count: state.count - 1 })), 
		resetState: () => set(initialState) 
	} 
}))
```

전체 상태를 초기화하는 것에 더해 일부 상태도 초기화하려면, 다음과 같이 `resetState` 함수를 수정할 수 있다.

```tsx
import { create } from 'zustand' 

interface State { 
	count: number 
	double: number 
	min: number 
	max: number 
} 

interface Actions { 
	actions: { 
		increase: () => void 
		decrease: () => void 
		resetState: (keys?: Array<keyof State>) => void
	} 
} 

const initialState: State = { 
	count: 1, 
	double: 2, 
	min: 0, 
	max: 99 
} 

export const useCountStore = create<State & Actions>(set => ({ 
	...initialState, 
	actions: { 
		increase: () => set(state => ({ count: state.count + 1 })), 
		decrease: () => set(state => ({ count: state.count - 1 })), 
		resetState: () => keys => {
			//전체 상태 초기화
			if (!keys) {
				set(initialState)
				return
			}
			//일부 상태 초기화
			keys.forEach(key => {
				set(({ [key]: initialState[key] }))
			})
		} 
	} 
}))
```

그리고 다음과 같이 호출한다.

```tsx
import { useCountStore } from './store/count' 
	
export default function resetState() { 
	const { resetState } = useCountStore(state => state.actions) 
	return (
		<> 
			<button onClick={() => resetState()}> 
				Reset All! 
			</button> 
			<button onClick={() => resetState(['double', 'min'])}> 
				Reset Double, Min! 
			</button> 
		</>
	)
}
```

### 상태의 타입추론

상태의 타입을 직접 사용하지 않고 추론하도록 `combine` 미들웨어를 사용할 수 있다 `combine`
미들웨어는 첫 번째 인수로 추론할 상태를 받고, 두 번째 인수로 `set`, `get` 매개변수를 포함하는 액션 함수를 받는다.

```tsx
import { combine } from 'zustand/middleware'

const state = { ...상태 }
const action = (set, get) => ({ ...액션 })
combine(state, actions)
```
