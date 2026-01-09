
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

컴퓨터는 IP 주소를 통해 통신하므로 브라우저는 서버에 연결하고 HTTP 요청을 보내기 전에 먼저 DNS 시스템에 도메인 이름을 IP 주소로 변환해 달라고 요청한다.

![](https://i.imgur.com/aV6sw8k.png)

### TCP 연결 설정 - Establishing the TCP connection

DNS가 브라우저에 IP 주소를 할당한 후에도 브라우저는 서버와의 안정적ㅇ니