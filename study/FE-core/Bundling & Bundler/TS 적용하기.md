---
title: TypeScript 적용하기
source: https://frontend-fundamentals.com/bundling/webpack-tutorial/typescript.html
---
TS는 JS에 타입 시스템을 추가한 언어임. 하지만 브라우저는 TS를 직접 실행할 수 없기 때문에, TS 코드는 반드시 JS로 컴파일해야 함.

---
### 웹팩이 TS를 이해하는법: 로더(Loader)

웹팩은 기본적으로 `.js`와 `.json`만 이해할 수 있음.
그래서 `.ts`나 `.scss`, `.png`같은 파일을 처리하면 [[로더]]라는 도구가 필요함. 로더는 웹팩에게 통역사 역할을 함.

`ts-loader`라는 로더가 TS 파일을 웹팩이 이해할 수 있는 JS로 변환함.

---
### TS 개발 환경 설정

먼저 TS를 웹팩이 사용할 수 있도록 관련 패키지를 설정.

```bash
npm install --save-dev typescript ts-loader
```

- `typescript`: TS 언어 자체와 컴파일러 포함.
- `ts-loader`: 웹팩이 `.ts`파일을 읽을 수 있도록 도와주는 