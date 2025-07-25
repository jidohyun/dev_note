

---


## 자바스크립트는 인터프리터 언어일까, 컴파일러 언어일까? 깊이 있게 살펴보자

자바스크립트가 인터프리터 언어인지 컴파일러 언어인지에 대한 논쟁은 오랫동안 이어져 왔다. 이는 단순히 용어의 문제를 넘어, 언어의 특성과 실행 방식에 대한 깊이 있는 이해를 필요로 한다.

**왜 논쟁이 일어났을까?**

- **과거의 오해:** 스크립트 언어는 컴파일 언어에 비해 성능이 낮고, 동적 타이핑으로 인해 안정성이 떨어진다는 인식이 강했다.
- **배포 방식의 차이:** 컴파일 언어는 바이너리 파일로 배포되는 반면, 자바스크립트는 소스 코드 자체를 배포한다.
- **실행 방식의 다양성:** 현대 프로그래밍 언어는 다양한 실행 방식을 지원하며, 단순히 인터프리터 또는 컴파일러로 분류하기 어려운 경우가 많다.

**자바스크립트는 어떻게 실행될까?**

자바스크립트 엔진은 소스 코드를 실행하기 전에 몇 가지 단계를 거친다.

1. **파싱:** 소스 코드를 토큰으로 분해하고 구문 분석하여 추상 구문 트리(AST)를 생성한다.
2. **컴파일:** AST를 실행 가능한 코드로 변환한다. 이 과정에서 최적화가 이루어질 수 있다.
3. **실행:** 생성된 코드를 실행한다.

**핵심은 바로 JIT(Just-In-Time) 컴파일이다.** 코드가 실행되는 순간에 실시간으로 컴파일되고 최적화되는 방식이다.

**왜 자바스크립트를 인터프리터 언어라고 부를까?**

- **소스 코드를 직접 실행하는 것처럼 보인다.**
- **동적 타이핑을 지원한다.**
- **스크립트 언어의 특징을 많이 가지고 있다.**

**왜 자바스크립트를 컴파일러 언어라고 부를 수 있을까?**

- **파싱과 컴파일 과정을 거친다.**
- **JIT 컴파일을 통해 최적화된다.**
- **일부 자바스크립트 엔진은 실행 전에 코드를 분석하여 오류를 사전에 감지한다.**

**결론적으로 자바스크립트는 인터프리터와 컴파일러의 특징을 모두 가지고 있는 하이브리드 언어라고 할 수 있다.**

- **인터프리터적 특징:** 소스 코드를 직접 실행하고, 동적 타이핑을 지원하며, 스크립트 언어의 특징을 가지고 있다.
- **컴파일러적 특징:** 파싱, 컴파일, 최적화 과정을 거치며, 정적 분석을 지원한다.

**따라서 자바스크립트를 단순히 인터프리터 언어 또는 컴파일러 언어로 분류하기보다는, 실행 환경과 사용 목적에 따라 다르게 평가해야 한다.**

**핵심:** 자바스크립트는 JIT 컴파일을 통해 높은 성능과 유연성을 동시에 제공하는 현대적인 프로그래밍 언어이다.

**요약:** 자바스크립트는 인터프리터와 컴파일러의 장점을 모두 가지고 있는 하이브리드 언어이며, 실행 환경과 사용 목적에 따라 다르게 평가될 수 있다.


![](https://i.imgur.com/Oyj683G.png)
(인터프리터, 스크립트 언어의 실행 절차)


![](https://i.imgur.com/0tIkhyH.png)
(파싱 + 컴파일 + 실행)


### 컴파일 단계에선 어떤 일이 일어날까?

컴파일 단계에선 가상 머신(VM)에 전달할 이진 바이트 코드가 생성된다. 혹자는 가상 머신의 역할이 전달받은 바이트 코드를 해석하는 것이라고 이야기한다. 그런데 이런 관점에서는 자바를 비롯한 JVM 기반 언어는 컴파일이 아닌 인터프리터 언어라고 해석할수밖에 없다. 그럼 자바 등의 언어가 컴파일 언어라는 보편적인 주장에 모순이 생긴다.

자바와 JS는 완전히 다른 언어이지만 인터프리터와 컴파일 관점에서는 비슷한 점이 많다는 게 상당히 흥미로운 지점이다.

또 다른 흥미로운 점은 JS 엔진은 파싱 이후 생성된 코드를 다양한 방법으로 실행 전에 그때그떄 처리 및 최적화 한다는 점이다.
이런 JS 엔진 작동 방식으로 인해 관점에 따라 JS를 컴파일 언어 혹은 인터프리터 언어라고 할 수 있는 것이다. JS 엔진 안에서는 우리가 상상할 수 없을 정도로 복잡한 일들이 일어나고 있다.

지금까지 정리한 지식들을 모아 결론을 내려보아 JS로 만든 소스 코드가 실행될 때까지 어떤 절차를 거치는지 정리해보면,

1. 개발자의 손을 떠난 코드는 바벨이 트랜스파일한다. 이후 웹팩을 비롯한 번들러를 거쳐 번들링되고, 그 결과가 JS엔진에 전달된다.
2. JS 엔진은 전달받은 코드를 파싱해 추상 구문 트리로 바꿔준다.
3. 이어서 인젠은 추상 구문 트리를 이진 바이트 코드로 바꾼다. 이 과정에서 JIT컴파일러가 작동하며 최적화가 함께 진행된다.
4. 마지막으로 JS 가상 머신이 프로그램을 실행한다.

네 단계를 그림으로 표현하면, 

### JS로 만든 소스 코드가 실행되는 과정

![](https://i.imgur.com/vdOpyE5.png)
(파싱, 컴파일 후 JS가 실행됨)

그렇다면 JS는 한 줄씩 실행되는 인터프리터 언어일까, 혹은 몇 단계를 거쳐 실행되는 컴파일 언어일까?

### 필자의 의견

완전한 사실이 아니긴 하지만, YDKJSY 책의 필자는 JS가 컴파일 언어라고 생각한다고 언급했다.

> JS를 컴파일 언어라고 생각하는 게 중요한 이유는 갸벌저거 아성헌 문법을 입력하는 등의 실수를 하더라도 코드 실행 전에 정적 오류를 미리 발견할 수 있다는 특징이 있기 때문이다. 이런 JS만의 방식은 기존 스크립트 프로그램과 상당히 다른 방식으로 개발할 수 있도록 한다.

