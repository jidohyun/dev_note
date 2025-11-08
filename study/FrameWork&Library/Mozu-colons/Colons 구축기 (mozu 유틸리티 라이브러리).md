
---

vitest 4.x 버전에선 3.x 버전까지 지원하던 기존의 파일명을 `vitest.workspace`로 바꾸거나, `defineWorkspace`를 사용하여 모노레포의 패키지를 구분하는 기능이 없어져있었다

4.x 버전부턴 `test.project` 라는 키를 사용해서 배열로 구분한다.

```ts
import { defineConfig } from 'vitest/config';

export default defineConfig({
	test: {
		projects: ['packages/*'],
	},
});
```

참고: https://vitest.dev/guide/projects.html

