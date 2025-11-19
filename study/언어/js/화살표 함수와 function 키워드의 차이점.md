### 개요

화살표 함수와 function 선언식의 차이는 **반환 방식**, **arguments 지원 여부**, **this 바인딩 방식**에서 뚜렷하게 드러난다.

### 핵심 차이

- **암시적 반환**: 화살표 함수는 코드 블록 없이 표현식만 쓰면 `return` 생략 가능.
- **arguments 여부**: function은 `arguments` 객체 사용 가능, 화살표 함수는 없어 `...args`로 대체.
- **this 바인딩**: function은 호출 방식에 따라 this가 달라지는 반면, 화살표 함수는 상위 스코프의 this를 고정 참조(렉시컬 this).

### 렉시컬 this 도입 이유

- function의 this는 호출 컨텍스트에 따라 달라 예측이 어려움 → 화살표 함수는 일관된 this로 혼동을 줄이기 위함.

### function의 this 요약

- 일반 호출: 전역 객체(window) / strict 모드에서는 undefined
- 객체 메서드 호출: 해당 객체
- 이벤트 핸들러: 이벤트가 발생한 요소
- ES6 이전에는 apply/call/bind로 this를 명시적으로 제어

### 면접용 한 줄 정리

“화살표 함수는 arguments가 없고 this를 상위 스코프에 렉시컬 바인딩하며 표현식은 암시적 반환이 가능하고, function은 arguments를 지원하며 호출 방식에 따라 this가 달라진다.”