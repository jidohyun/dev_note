
---

### 의도

JavaScript를 사용한 적이 없는 웹 개발자는 없을 것이다.
그만큼 JavaScript는 웹 개발을 하기 위해 필수적인 언어이고, 이젠 웹 개발을 넘어 백엔드, 네이티브, 심지어 임베디드 장치 등 다양한 분야에서 JavaScript를 사용한다.

JavaScript의 내부 작동 방식을 탐구해보고, 구성요소들이 어떻게 작동하는지 정리했다.

JavaScript에 크게 의존하는 프로젝트일 수록, 개발자는 더욱더 좋은 소프트웨어를 만들기 위해 내부 구조를 더욱더 깊이 이해하고, 언어가 제공하는 모든 것을 최대한 사용해야 한다고 생각한다. 

실제로 매일 JavaScript를 사용하는 사람 중에서도, 내부에서 어떤 일이 일어나는지 모르는 사람들이 많다.

### JavaScript 엔진

V8엔진은 익히 들어봤을 것이다. V8엔진은 현재 Chromium기반 인터넷 사용자의 약 75% 정도가 사용한다. (Chrome, Node.js에서 사용됨)

![](https://i.imgur.com/rP2uD3P.png)

JavaScript의 엔진은 이렇게 두 가지의 구성요소로 구성된다.
- Memory Heap: 메모리 할당이 발생하는 위치
- Call Stack: 코드가 실행될 때 스택 프레임이 있는 위치

### 런타임

브라우저엔 거의 모든 JavaScript 개발자가 사용한 API가 있다. ex) `setTimeout`
그러나 이런 API들은 엔진에서 제공하지 않는다.

좀 더 복잡한 형태로 제공되는데,

![](https://i.imgur.com/DN52Oun.png)


물론 JavaScript는 엔진으로 동작하지만, 실제로는 훨씬 더 많은 것들이 있다.
브라우저에서 제공하는 웹 API라는 것들을 가지고 있는데, DOM, AJAX, setTimeout등이 있다.

그리고, 이벤트 루프와, 콜백 큐가 있다.

### Call Stack(호출 스택)

JavaScript는 단일 스레드 프로그래밍 언어이다. 즉, 단일 호출 스택이 있다. 따라서 **한 번에 한 가지 작업을 수행할 수 있다.** 

**호출 스택은, 프로그램이 어느 위치에 있는지 기록하는 데이터 구조이다.** 함수에 들어가면 함수를 스택 맨 위에 푸쉬한다. 함수를 리턴하면 스택 맨 위에서 팝한다. 스택의 역할은 그게 전부이다.

다음 코드를 보면,

```js
function multiply(x, y) {  
    return x * y;  
}

function printSquare(x) {  
    var s = multiply(x, x);  
    console.log(s);  
}

printSquare(5);
```

엔진이 이 코드를 실행하기 시작하면, 호출 스택이 비어 있을 것이다. 그 후 단계는 다음과 같다.

![](https://i.imgur.com/60BZupV.png)

호출 스택의 각 항목을 **스택 프레임**이라고 한다. 

그리고 아래는 예외가 throw되었을 때 스택 추적이 구성되는 방식이다 기본적으로 