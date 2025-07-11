
---

### electron에 대한 렌더러, 프로세스의 심층분석

> [!NOTE]
> electron의 다중 프로세스 아키텍쳐 이해하기

Electron의 핵심은 두 개 이상의 프로세스가 동시에 실행된다는 개념인데 "main"과 "renderer"프로세스이다.
브라우저 JS를 다루던 사람들은 이런 프로세스를 다루는 것은 새로운 영역일 것이다. 

처음엔 확실한 패러다임의 전환이였고, 여러 프로세스로 작업한다는 것은 앱에서 다른 경우 하지 않았을 다른 디자인 선택을 한다는 것을 의미한다.

이쯤되면 궁금증이 생긴다

- Electron이 왜 이런 멀티 프로세스 아키텍쳐를 가지고 있을까?
- 메인 프로세스의 책임은 무엇일까?
- 렌더러 프로세스의 책임은 무엇일까?
- 어떻게 서로 통신할까?

### 프로세스?

프로세스의 대한 설명은 [[프로세스]] <<여기서

실제로 일렉트론을 실행하면 다중 프로세스인걸 확인할 수 있다.
왜 다중 프로세스로 만들었을까?