
----

> [!info]
> 이 포스트는 JS 전공도서인 YDKJSY 시리즈의 1장을 읽고 정리한 내용이다.

### 하위 호환성과 상위 호환성

JS를 지탱하는 기본 원칙 중 하나는 **하위 호환성**보장이다.
많은 개발자들은 이 **하위 호환성**과 **상위 호환성**이란 개념을 혼동한다.

#### 하위 호환성

하위 호환성이란, 단 한 번 이라도 유효한 JS문법이라고 인정되면 명세서가 변하더라도 절대 유효성이 깨지지 안는다는 의미이다. 예를 들어 하위 호환성 덕분에 1995년에 작성한 코드가 지금 실행해도 작동한다는걸 보장받는다.
#### 상위 호환성

상위 호환성이란, **새로운 명세서에 추가된 문법으로 코드를 작성했을 때 이전 명세서를 준수하는 구형 JS엔진에서 문제가 발생하지 않아야 한다**. 
많은 개발자들은 JS가 상위 호환성을 보장한다고 생각하지만 JS는 상위 호환성을 보장하지 않는다. 상위 호환성을 보장하는 기술의 예로는 HTML, CSS가 있다. 1995년에 작성된 HTML과 CSS를 어디선가 찾아냈다면 그 코드는 오늘날에 작동하지 않을 수 있다.

### 트랜스파일러?

JS엔 트랜스파일이라는 개념이 있다.
트랜스파일이란, JS 커뮤니티에서 만든 용어로, `한 형태에서 다른 형태로 소스코드를 변환해주는 것`을 의미한다.
이때, 변환 후 산출물은 소스코드이다. **상위 호환성**으로 인해 발생하는 문제 대부분은 트랜스파일러를 사용하면 해결된다. 트랜스파일러는 새로운 JS 문법을 오래된 문법으로 바꿔주며 주로 **babel**을 사용한다.

트랜스파일러가 어떻게 작동하는지의 대한 예시이다.
아래는 **ES6 (ECMAScript 2015)** 로, 비교적 최근 추가된 문법이다

```js
if (something) {
	let x = 3;
	console.log(x);
}
else {
	let x = 4;
	console.log(x);
}
```

다음은 앞선 코드를 [[Babel]]이 트랜스파일한 결과이다. 프로덕션 환경에 배포할 때는 이처럼 트랜스파일러를 거친 산출물(js 파일)을 배포한다.

```js
var x$0, x$1;
if(something) {
	x$0 = 3;
	console.log(x$0);
}
else {
	x$1 = 4;
	console.log(x$1);
}
```

이렇게 babel을 사용해 트랜스파일한 아래 코드의 작동 결과는 원래 코드와 동일하다.

> [!NOTE]
> let 키워드는 ES6에서 추가된 문법이므로 ES6 이전 문법을 지원해야 하는 환경에선 앞선 예시처럼 트랜스파일이 꼭 필요하다.

어차피 var로 트랜스파일 하는데 굳이 let을 쓸 필요가 있는가? 
최신 JS를 쓰는게 좋다고 강하게 권유하는 이유는 코드가 깔씀해지고 의도하는 바를 효과적으로 전달할 수 있기 때문이다.

개발자는 새로운 문법을 활용해 **클린한 코드를 짜는데 집중해야한다**. 상위 호환성과 관련된 문제는 **babel과 같은 도구에 맡겨** 오래된 JS환경에서도 별도 처리 없이 정상적으로 코드가 실행되도록 만들면 된다.

