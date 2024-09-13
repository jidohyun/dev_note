
---

### 상태관리 라이브러리?

우리는 react hook인 useState를 주로 사용하여 상태관리를 한다.
하지만 중요한 변수가 늘어날 수록 useState의 상태관리가 어려워지는데..

이렇게 많은 상태를 관리하기 편하게 해주는 라이브러리가 있는데..
그의 이름은 zustand이다.

### 사용법

```ts
// store.ts 
import create from "zustand";

// store의 타입을 정의해준다.
interface Store { 
	data : string;
	setData : () => void;
} 

// store를 create
const useStore = create<Store>((set) => ({
	data : '', 
	setData : (newData) => set((state) => ({data : newData})
}));

export default useStore;
```

이렇게 쉽게 스토어를 생성해주었다.