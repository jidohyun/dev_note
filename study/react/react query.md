
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
		queryFn: asyn
	})
}
```