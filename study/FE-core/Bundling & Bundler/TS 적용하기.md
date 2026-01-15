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
- `ts-loader`: 웹팩이 `.ts`파일을 읽을 수 있도록 도와주는 로더임.

---
### TS 설정 파일 만들기
웹팩 설정 파일로 `webpack.config.js`가 있듯이, TS도 컴파일러에게 코드를 어떻게 처리할지 알려주는 설정 파일이 있음.

루트 폴더에 다음과 같이 `tsconfig.json`파일을 만듬.

```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "esnext",
    "strict": true,
    "skipLibCheck": true,
    "moduleResolution": "node"
  },
  "include": ["./**/*.ts"],
  "exclude": ["node_modules", "dist"]
}
```

> tsconfig 옵션 뜻
> ![](https://i.imgur.com/0gItJFJ.png)

---
### 웹팩에 로더 설정 추가하기

이제 웹팩에게 `.ts` 파일을 어떻게 처리할지를 알려줌. `ts-loader`는 TS 파일을 JS로 변환할 때 아까 만들었던 `tsconfig.json`의 설정을 참고.

`webpack.config.js`를 다음과 같이 수정.

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './main.ts', // 웹팩이 읽기 시작할 파일을 .ts로 변경했어요.
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.ts$/, // .ts 파일들은
        use: 'ts-loader', // ts-loader를 거쳐 처리돼요.
        exclude: /node_modules/ // 외부 모듈은 제외해요.
      }
    ]
  },
  resolve: {
    extensions: ['.ts', '.js'] // 파일을 import할 때 확장자를 생략할 수 있어요. TypeScript와 JavaScript를 혼용하는 프로젝트에서 설정해두면 좋아요.
  }
};
```

---
### JS 파일을 TS로 변환하기

기존 JS파일들을 