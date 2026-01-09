
link: https://howbrowserswork.com/

### 브라우저는 URL을 사용해서 작동한다 - Browsers work with URLs

브라우저 검색창에는 무엇이든 입력할 수 있다. 하지만 브라우저 내부적으론 URL을 사용하여 동작함.

- `피자`와 같은 임의의 텍스트는 `https://google.com/search?q=pizza`와 같이 "search" URL로 정규화된다 (브라우저 기본 검색엔진에 따라서 `https://duckduckgo.com/?q=pizza`)
- `example.com`과 같은 도메인은 `https://example.com`과 같은 전체 URL로 정규화된다.

![](https://i.imgur.com/VniXfx2.png)

![](https://i.imgur.com/AIvzMbt.png)
### URL을 HTTP 요청으로 변환하기 - Turning URL into an HTTP request

방문하고자 하는 URL을 알게 되면, 서버에 요청을 보내 해당 리소스를 가져와 브라우저에 표시할 수 있다.
브라우저는 HTTP 프로토콜을 사용하여 서버와 통신한다.

![](https://i.imgur.com/6YjZFA8.png)

HTTP요청 헤더는 다음과 같은 형식임.

```
Host: naver.com
Accept: text/html
```

헤더중 하나는 호스트 헤더다. 이는 요청이 전송되는 서버에 (예: `naver.com`)을 식별하는 데 사용된다.

### 서버 주소 확인중 - Resolving the server address

브라우저는 `example.com`과 같은 이름으로 요청을 보낼 수 없다.

컴퓨터는 IP 주소를 통해 통신하므로 브라우저는 서버에 연결하고 HTTP 요청을 보내기 전에 먼저 [[DNS]] 시스템에 도메인 이름을 IP 주소로 변환해 달라고 요청한다.

![](https://i.imgur.com/aV6sw8k.png)

### TCP 연결 설정 - Establishing the TCP connection

DNS가 브라우저에 IP 주소를 할당한 후에도 브라우저는 서버와의 안정적인 연결이 필요하다. TCP는 HTTP 데이터가 전송되기 전에 이러한 연결을 설정하는 프로토콜이다. TCP는 양측이 데이터를 송수신 할 준비가 되어있는지 확인하는 [[three-way handshake]]를 통해 연결을 설정한다.

![](https://i.imgur.com/dw9xjie.png)

이 숫자들은 (SYN, ACK) 클라이언트와 서버가 통신 과정을 추적하는 데 사용된다. 바이트 수를 세어 양측이 데이터 스트림의 시작 위치와 다음에 데이터를 확정할 수 있도록 한다. 만약 데이터가 누락되면 송신자는 누락된 부분을 확인하고 해당 바이트를 재전송 할 수 있다. TCP는 이러한 방식으로 연결이 설정된 후 데이터의 순서와 신뢰성을 유지한다.

### HTTP요청 및 응답 - HTTP requests and responses

TCP 연결이 설정되면 브라우저는 서버에 HTTP 요청을 보낼 수 있다.

![](https://i.imgur.com/hTb5iJB.png)

HTTP응답이 도착하면 브라우저는 원시 HTTP 응답을 읽고 HTML콘텐츠 렌더링을 시작한다.

### HTML을 파싱하여 DOM트리를 구성한다 - Parsing HTML to build the DOM tree

HTTP 응답이 도착하면 브라우저는 헤더와 본문을 분리하고 HTML 바이트를 파서에 입력한다. 파서는 `<h1>`과 같은 태그를 토큰으로 변환하고 DOM 트리를 구축한다. 


![](https://i.imgur.com/tflxe4H.png)

구문 분석은 스트리밍 방식으로 처리되며 오류를 허용한다. 브라우저는 전체 문서가 다운로드되기 전에 노드 생성을 시작하고, 누락된 태그를 삽입하여 트리의 유효성을 유지한다. `<script>`태그가 나타나면 실행될 수 있도록 구문 분석이 일시 중지될 수 있다

DOM 트리는 CSS와 레이아웃과 페인트가 픽셀을 그리는데 사용되는 렌더링 트리를 생성한다.

### DOM 중요성에 대하여 - On the importance of the DOM

`DOM`은 브라우저 메모리에 저장된 문서 모델이다. HTML 파서, CSS 선택 엔진, Javascript 런타임 간의 공유 계약 역할을 하므로 DOM을 변경하면 레이아웃, 스타일, 사용자가 상호 작용할 수 있는 요소에 즉시 영향을 미침.

DOM은 쿼리 선택부터 스타일링 이벤트 처리까지 모든 것을 지원한다.

![](https://i.imgur.com/TRK0T1F.png)

### 레이아웃, 페인트 및 합성 - Layout, Paint, and Composite

DOM과 CSS가 준비되면 브라우저는 렌더링 파이프라인을 실행한다. `Layout`(reflow) 단계에서는 크기와 위치를 계산하고, `Paint` 단계에선 픽셀을 채운 다음, `Composite` 단계에선 GPU에서 레이어를 결합한다.

모든 변경 사항이 모든 단계를 다시 실행하는 것은 아니다. 색상을 변경하면 일반적으로 화면이 다시 그려지고, 크기를 변경하면 레이아웃과 화면 그리기가 다시 계산된다.

레이아웃이 복잡한 페이지가 느려지는 이유가 이것 때문이다. 다음 프레임을 표시하기 전에 더 많은 작업이 필요하기 때문이다.

