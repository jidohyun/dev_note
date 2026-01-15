---
title: 웹팩 도입하고 첫 번들 만들기
source: https://frontend-fundamentals.com/bundling/webpack-tutorial/make-first-bundle.html#%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%91%E1%85%A2%E1%86%A8-%E1%84%83%E1%85%A9%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%92%E1%85%A1%E1%84%80%E1%85%A9-%E1%84%8E%E1%85%A5%E1%86%BA-%E1%84%87%E1%85%A5%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF-%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5
---
### Webpack이란?

웹팩은 가장 널리 사용되는 대표적인 번들러임. 번들러에 대한 자세한 설명은 [[Bundler?]] 문서 참고.

웹팩의 가장 기본적인 기능은 개발자의 편의와 생산성을 위해 여러 개로 나눠서 만든 JS 파일과 CSS, 이미지 같은 리소스를 묶어서, 브라우저가 이해할 수 있는 하나의 번들 파일로 만들어주는 것.

![](https://i.imgur.com/CivaZ3Z.png)

---
### 웹팩 설치하기

웹팩과 웹팩 CLI 개발 의존성 설치

```bash
npm install --save-dev webpack webpack-cli
```

- `webpack`: 실제 번들링을 수행하는 도구임.
- `webpack-cli`: 터미널에서 webpack 명령어를 쓸 수 있게 해주는 CLI.

웹팩은 번들링 도구로, 실제 배포되는 코드에서는 필요 없기 때문에 `--save-dev` 옵션을 통해 개발 의존성으로 설치함.

---
### 웹팩 설정 파일 만들기

`main.js` 파일을 번들링해서 `bundle.js`로 만든다.

먼저 프로젝트 루트에 `webpack.config.js`이름의 파일을 생성하고 다음과 같이 작성함. 이 설정 파일을 보고 웹팩은 `main.js`파일을 번들링해서 `bundle.js` 파일로 만듬.

```js
const path = require("path");

module.exports = {
  entry: "./main.js", // 어떤 파일을 진입점으로 번들링할지
  output: {
    filename: "bundle.js", // 번들로 만들어질 파일 이름
    path: path.resolve(__dirname, "dist") // 번들 파일이 어디에 저장될지
  }
};
```

---
### 첫 번들 만들기

웹팩으로 번들을 만든다. 아래 명령어 실행시 빌드 시작됌.

```bash
npx webpack
```

빌드가 완료되면 `dist` 폴더 안에 `bundle.js` 파일이 생성됌.

> TIP
> `npx`는 Node.js와 함께 설치된 도구로, 로컬에 설치된 패키지를 실행할 수 있게 해줌.

`/dist/bundle.js`를 열어보면 `main.js`에 적었던 코드가 한 줄로 압축되고, 변수명이 `t`, `e`같은 짧은 이름으로 바뀐걸 알 수 있음. 이렇게 웹팩은 추가 설정을 하지 않아도 기본적으로 코드를 최적화 해줌.

```js
document.addEventListener("DOMContentLoaded", function () {
  const t = new Date(),
    e = dateFns.format(t, "MMMM d, yyyy");
  ((document.getElementById("dateDisplay").textContent = e),
    (function () {
      const t = Math.floor(Math.random() * emojis.length),
        e = emojis[t];
      ((document.getElementById("emojiDisplay").textContent = e.icon),
        (document.getElementById("emojiName").textContent = e.name));
    })());
});
```

---

### 