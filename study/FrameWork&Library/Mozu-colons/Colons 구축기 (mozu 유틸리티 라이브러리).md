
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

>Vitest는 내부에 구성 파일이 없더라도 `패키지의` 모든 폴더를 별도의 프로젝트로 취급합니다. glob 패턴이 파일과 일치하면 이름이 `vitest.config`/`vite.config`로 시작하는지 또는 `(vite|vitest).*.config.*` 패턴과 일치하는지 확인하여 Vitest 구성 파일인지 확인합니다. 예를 들어 다음 구성 파일은 유효합니다.

추가로 패키지 내의 vitest 세팅은 vitest가 glob 패턴으로 파일을 확인해서 vitest 세팅 파일인지 확인한다.

```
// ex: 아래 파일 모두 유효함
- `vitest.config.ts`
- `vite.config.js`
- `vitest.unit.config.ts`
- `vite.e2e.config.js`
- `vitest.config.unit.js`
- `vite.config.e2e.js`
```

