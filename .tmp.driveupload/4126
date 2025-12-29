
Babel은 **자바스크립트 컴파일러**이다. 왜 인터프리터 언어에 컴파일러가 필요하지? 라고 생각하는 사람도 있겠지만,

정확히 Babel은 Javascript로 결과물을 만들어주는 컴파일러이다. 
**소스 대 소스 컴파일러 (transpiler)** 라고 불린다.

### 왜?

그렇다면 왜 Javascript 로 변환하는 과정이 필요할까?

지금도 프론트엔드는 너무나 빠르게 발전되고 있다.
심지어 최신 브라우져 조차도 지원하지 못하는 문법과 여러 기술들이 출연하고 있다.

새로운 ESNext 문법을 기존의 브라우져에 사용하기 위해서 babel은 필수적이다.
모든 사람들이 새로운 브라우져를 쓰면 좋겠지만 슬프게도 아직도 많은 사람들이 예전 브라우져 예전 OS를 사용하고 있다.

때문에 Typescript 든 CoffeeScript 든 Javascript 로의 Compile이 필수가 되어야 하며,
이를 담당하는게 Babel이다.

### Polyfill?

폴리필은 개발자가 특정 기능이 지원되지 않는 브라우저를 위해 사용할 수 있는 코드 조각이나 플러그인을 의미 한다.

브라우저에서 지원하지 않는 기능들에 대한 호환성 작업을 채워 넣는다고 해서 Polyfill이라고 칭한다.

### babel-polyfill

babel은 이러한 polyfill을 손쉽게 지원하기 위해 babel-polyfill 기능을 지원한다.
아까 이미 문법을 컴파일해서 Javascript로 Compile 한다고 했는데 왜 polyfill이 필요할까?

babel을 사용한다고 최신 함수를 사용할 수 있는 건 아니다.
babel은 문법을 변환하여 javascript 로 변환하는 transpiler 역할만 할 뿐이다.

앞에서 설명한대로 polyfill은 프로그램이 처음에 시작될 때 지원하지 않는 기능들을 추가하는 것이다.
즉, babel은 컴파일시에 실행되고 babel-polyfill은 런타임에 실행되는 것이다.

얼마나 쉽게 설정할 수 있냐하면,
아래는 최근 브라우져 2가지의 버전만 지원하면서 IE의 경우 10버전 이하는 제외하는 설정을 한 것 이다.

```json
{
	"presets": [
	    ["env", {
		    "targets": {
	            "browsers": ["last 2 versions", "not ie <= 10"]
	        }
        }]
      ]
    }
```

