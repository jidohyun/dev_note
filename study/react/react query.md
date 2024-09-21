
---

### react query?

react query는 서버로부터 데이터 가져오기, 데이터 캐싱, 캐시 데이터 관리 등 데이터를 쉽게 관리 할 수 있는 라이브러리이다.

대표적인 기능
- 데이터 가져오기 및 캐싱
- 동일 요청의 중복 제거
- 신선한 데이터 유지 
- 무한 스크롤, 페이지네이션 등의 성능 최적화
- 네트워크 재연결, 요청 실패 등의 자동 갱신

### 데이터 캐싱

react query를 사용하여 데이터를 가져올땐 항상 쿼리 키(`queryKey`)를 지정하게 된다. 
이 쿼리 키는 캐시된 데이터와 비교해 새로운 데이터를 가져올지, 캐시된 데이터를 사용할지 결정하는 기준이 된다.

```tsx
import { useQuery } from '@tanstack/react-query' 

export default function DelayedData() { 
	const { data } = useQuery({ 
	queryKey: ['delay'], 
	queryFn: async () => (await fetch('https://api.heropy.dev/v0/delay?t=1000')).json()
	})
	return <div>{JSON.stringify(data)}</div> 
}
```

다음 이미지는 쿼리 키와 일치하는 데이터가 없을 때, 서버에서 새로운 데이터를 가져오는 과정을 보여준다.
서버에서 데이터를 가져오면 그 데이터는 캐시되고 그 이후 요청부터는 캐시된 데이터를 사용할 수 있다.

![](https://i.imgur.com/OECYMfJ.png)
*캐시된 데이터가 없을 때*

반대로 쿼리 키와 일치하는 데이터가 있으면, 서버에 요청하지 않고 캐시된 데이터를 사용하게 된다.
따라서 같은 데이터를 가져오는 요청이 여러 번 발생해도, 캐시된 데이터를 사용하게 되어 중복 요청을 줄일 수 있다. 

![](https://i.imgur.com/sO3wUCk.png)
*캐시된 데이터가 있을 때*

### 데이터의 신선도

react query는 캐시한 데이터를 신선하거나 상한 상태로 구분해 관리한다.
캐시된 데이터가 신선하다면 캐시된 데이터를 사용하고, 만약 데이터가 상했다면 다시 서버에 데이터를 요청해서 신선한 데이터를 가져온다.
*일종의 데이터 유통기한*

데이터가 상하는 데 걸리는 시간은 `staleTime`옵션으로 지정할 수 있다.
그리고 신선한지 상했는지 여부는 `isStale`으로 확인할 수 있다.

```tsx
import { useQuery } from '@tanstack/react-query'

export const function DelayedData() {
	const { data, isStale } = useQuery({
		queryKey: ['delay'],
		queryFn: // 데이터 호출
		staleTime: 1000 * 10 //10초 후 상함 즉, 10초 동안 신선함.
	})
	return (
		<>
			<div>데이터가 {isStale ? '상했어요..' : '신선해요!'}</div>
			<div>{JSON.stringify(data)}</div>
		</>
	)
}
```

### useQuery

가장 기본적인 쿼리 훅으로, 컴포넌트에서 데이터를 가져올 때 사용한다.

```tsx
const 반환 = useQuery
```

지연 응답 API는 `t`파라미터 값의 시간이 지난 후 응답한다.
응답 데이터는 간단한 메시지와 응답시간을 포함한다.

```tsx
import { useQuery } from '@tanstack/react-query'

type ResponseValue = {
	message: string
	time: string
}

export default function DelayedData() {
	const { data } = useQuery<ResponseValue>({
		queryKey: ['delay'],
		queryFn: // api 호출
		staleTime: 1000 * 10 // 10초
	})
	return <div>{data?.time}</div>
}
```

### queryKey

쿼리 키는 쿼리를 식별하는 고유한 값으로, 배열 형태로 저장한다.
다중 아이템 쿼리 키를 사용할 때는 아이템의 순서가 중요하다.

```tsx
// 단일 아이템 쿼리 키 
useQuery({ queryKey: ['hello'] }) 

// 다중 아이템 쿼리 키 
useQuery({ queryKey: ['hello', 'world', 123, { a: 1, b: 2 }] }) 

// 서로 같은 쿼리 
useQuery({ queryKey: ['hello', 'world', 123, { a: 1, b: 2 }] }) 
useQuery({ queryKey: ['hello', 'world', 123, { b: 2, c: undefined, a: 1 }] }) 

// 서로 다른 쿼리 
useQuery({ queryKey: ['hello', 'world', 123, { a: 1, b: 2 }] }) 
useQuery({ queryKey: ['hello', 'world', 123, { a: 1, b: 2, c: 3 }] }) 
useQuery({ queryKey: ['hello', 'world'] }) 
useQuery({ queryKey: [123, 'world', { a: 1, b: 2, c: 3 }], 'hello' })
```

다음 예제에서 DelayedData 컴포넌트의 `wait` Prop의 값이 다르면, 각각 별개의 요청을 전송한다.

```tsx
import { useQuery } from '@tanstack/react-query' 

type ResponseValue = { 
	message: string 
	time: string 
} 

export default function DelayedData({ wait = 1000 }: { wait: number }) { 
	const { data } = useQuery<ResponseValue>({ 
		queryKey: ['delay', wait], 
		queryFn: async () => (await fetch(`https://api.heropy.dev/v0/delay?t=${wait}`)).json(), 
		staleTime: 1000 * 10 
	}) 
	return <div>{data?.time}</div> 
}
```

```tsx
import { QueryProvider } from './queryProvider' 
import DelayedData from './components/DelayedData' 

export default function App() { 
	return ( 
	<QueryProvider> 
		<DelayedData /> 
			<DelayedData wait={2000} /> 
			<DelayedData wait={3000} /> 
		</QueryProvider> 
	) 
}
```

기본적으로 쿼리 함수(queryFn)에서 사용하느 변수는 쿼리 키에 포함돼야 한다.
그러면 변수가 변경될 때마다 자동으로 다시 가져올 수 있다.

### queryFn

쿼리 함수(queryFn)는 데이터를 가져오는 비동기 함수로, 꼭 데이터를 반환하거나 오류를 던져야 한다.
던져진 오류는 반환되는 `error`객체로 확인할 수 있다.
`error`는 기본적으로 `null`이다.

```tsx
import { useQuery } from '@tanstack/react-query' 

type ResponseValue = { 
	message: string 
	time: string 
} 

export default function DelayedData() { 
	const { data, error } = useQuery<ResponseValue>({ 
		queryKey: ['delay'], 
		queryFn: async () => { const res = await fetch('https://api.heropy.dev/v0/delay?t=1000') 
		const data = await res.json() 
		if (!data.time) { 
			throw new Error('문제가 발생했습니다!') 
		} 
		return data 
	}, 
	staleTime: 1000 * 10, 
	retry: 1 
	}) 
	return ( 
		<> 
			{data && <div>{JSON.stringify(data)}</div>} 
			{error && <div>{error.message}</div>} 
		</> 
	) 
}
```

### select

선택 함수(`select`)를 사용하면 가져온 데이터를 변형할 수 있다
쿼리 함수가 반환하는 데이터를 인수로 받아 선택함수에서 처리하고 반환하면 최종 데이터가 된다.
최종 데이터 타입은 `useQuery`의 3번째 제네릭 타입으로 선언할 수 있다.

```tsx
import { useQuery } from '@tanstack/react-query' 

type Users = User[] 
interface User { 
	id: string 
	name: string 
	age: number 
} 

export default function UserNames() { 
	const { data } = useQuery<Users, Error, string[]>({ 
		queryKey: ['users'], 
		queryFn: async () => { const res = await fetch('https://api.heropy.dev/v0/users') 
		const { users } = await res.json() 
		return users 
	}, 
	staleTime: 1000 * 10, 
	select: data => data.map(user => user.name) 
	}) 
	return ( 
		<> 
			<h2>User Names</h2> 
			<ul>{data?.map((name, i) => <li key={i}>{name}</li>)}</ul> 
		</> 
	) 
}
```

쿼리 함수를 따로 선언해 제공하면, 선택 함수를 통한 최종 데이터의 타입을 추론할 수 있다.
다음 예제에서 쿼리 함수의 반환은 `Users`타입이고, 최종 데이터(선택 함수의 반환)은 `string[]`타입으로 추론된다.

```tsx
// ... 
async function queryFn(): Promise<Users> { 
	const res = await fetch('https://api.heropy.dev/v0/users') 
	const { users } = await res.json() 
	return users 
} 

export default function UserNames() { 
	// data는 string[] 타입으로 추론 
	const { data } = useQuery({ 
	queryKey: ['users'], 
	queryFn, 
	staleTime: 1000 * 10, 
	select: data => data.map(user => user.name) 
	}) 
	// ... 
}
```

