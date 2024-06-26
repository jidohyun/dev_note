
---

### @keyframe

키 프레임을 활용하기 위해선 우선 css로 키프레임 블록을 만들어야 한다.

```css
@keyframes 애니메이션이름 {
	0% {            /* from 이라고 작성해도 됩니다.*/
		CSS속성 : 속성값; 
	}

	50% {           /* 애니메이션 진행도에 따른 스타일을 설정합니다. */
                  /* 필요하다면 1부터 99까지도, 소수점까지도 모두 작성해도 됩니다.*/
		CSS속성 : 속성값;
	}

	100% {          /* to 라고 작성해도 됩니다.*/
		CSS속성 : 속성값;
	}
}
```

해당 방법으로 회전하는 키 프레임 애니메이션을 만들어보면 다음과 같다.

```css
@keyframes lotate {
	0% {
		transform : rotate(0deg)
	}

	50% {
		transform : rotate(180deg)
	}

	100% {
		transform : rotate(360deg)
	}
}
```


### animation 속성

`animation` 에 전달해줄 수 있는 속성들은 다음과 같다.

- `animation` : 띄어쓰기로 쭉 나열하면 아래의 속성들을 한 번에 지정할 수 있다.
	- `animation-name`: 애니메이션의 중간 상태를 지정하는 이름. `@keyframes`블록에 작성
	- `animation-duration`: 한 싸이클의 애니메이션이 재생될 시간 지정
	- `animation-delay`: 애니메이션이 시작을 지연시킬 시간 지정
	- `animation-direction`: 애니메이션 재생 방향을 지정
	- `animation-iteration-count`: 애니메이션이 몇 번 반복될지 지정
	- `animation-play-state`: 애니메이션 재생 상태. 멈추거나 다시 재생시킬 수 있음
	- `animation-timing-function`: 중간 상태들의 전환을 어떤 시간간격으로 진행할지 지정
	- `animation-fill-mode`: 애니메이션이 재생 전 후의 상태 지정

### animation-name

애니메이션을 적용하고 싶은 요소에 `animation` 속성의 첫 번째 값으로, 혹은 `animation-name` 이라는 속성으로 `@keyframes` 키워드를 사용해서 만든 애니메이션 이름을 작성해주면 된다.