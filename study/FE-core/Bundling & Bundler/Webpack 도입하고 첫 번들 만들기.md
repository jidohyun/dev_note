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

- 