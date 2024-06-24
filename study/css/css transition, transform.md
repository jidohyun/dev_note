
---

### transition

트렌지션은 변화하는 단계의 중간 움직임을 생성하도록 도와준다.
예를들어, select메뉴에 커서를 올려놓았을 때 뚝뚝 끊기듯 서브메뉴가 펼쳐지는 것이 아니라 자연스럼게
시간차를 두고 펼쳐지도록 해준다 
**==<span style="background:#fff88f">속성적용은 최초상태의 태그에 한다</span>

- 기본구성 `transition: property duration timing-function delay;`
	- property : 변화를 일으킬 css 속성명
	- duration : 변화가 얼마동안 일어나게 할지 명시하는 지속시간 (1s=1000ms)
	- timing-function : 변화의 속도를 지정하는 가속도 (생략가능)
		- ease-in : 천천히 움직이다가 나중에 빠르게
		- ease-out : 천천히 움직이다가 나중에 천천히
		- ease-in-out : 천천히 빠르게 천천히
	- delay : 몇초 후에 동작할지 지정하는 지연시간

```html
<p class="box"><p>

<style>
    p.box{
    	width:200px;
        height:200px;
        background-color:red;
        transition: all 1s ease-in;	//모든 속성을 1초동안 ease-in
    }
    
    p.box:hover{
    	width:400px;
        height:200px;
        background-color: pink;
    }
    
    p.box:active{ 		//actvie - 클릭한 상태
    	width:300px;
        height:200px;
        background-color: pink;
    }
</style>
```

빨간색 박스에서 hover 시, 1초동안 파란박스로 색상이 자연스럽게 변화되어 간다.
그리고 박스를 active(클릭) 하고있는 동안, 핑크박스의 색깔과 크기로 변화한다. 

![](https://i.imgur.com/a7dNDZI.png)

- `transition : font-size 1s ease-out, background-color 2s ease-in 1s;`

### transform

초기값에 영향을 미치지 않는 요소의 변형

- 기본구성 `transform : scale(x,y) translate(x,y) rotate(deg) skew(xdeg,ydeg);`
	- translate(x,y) : 원래 위치를 기준으로 내가 원하는 방향으로 위치시킨다.
	- scale(x,y) : (숫자)의 배율만큼 사이즈를 조정한다. (1=기본값, 1.2=120%, 2=2배) 
	  *x,y축의 배율이 같으면 한번만 적어준다.*
	- rotate(deg) : 각도를 나타내는 deg(degree)를 넣어 시계방향으로 회전
	- skew(deg) : 비틀기 효과, 마름모 꼴.
- transform-origin : rotate(), skew() 등의 회전, 변형 속성을 사용하기 전에 기준점을 지정해 주는
  속성. 초기 값은 50% 50%으로 요소의 중심점이 된다. 백분율과 키워드로 작성 가능 (0% 0% = left top)

```html
<div id="wrap">
    <article>
        <p class="one"></p>
    </article>
    <article>
        <p class="two"></p>
    </article>  
    <article>
        <p class="three"></p>
    </article>  
</div>

<style>
	#wrap{
            width: 800px;
            margin: 30px auto;
        }

        article{
            width: 200px;
            height: 200px;
            margin: 0 0 80px 200px;
            border: 5px solid orange;
        }

        article>p{
            width: 200px;
            height: 200px;
            opacity: 0.5;
        }

        p.one{
      	    background-color: #0f0;
            transform: scale(1.2,1.2) skew(10deg);  //1.2(120%) 확대, skew(마름모꼴) 변형 
        }

        p.two{
            background-color: pink;
            transform-origin: right top;  //오른쪽 위를 기준으로 설정
            transform:scale(1.2) rotate(45deg);  //1.2배 확대, 45도 시계방향회전
        }

        p.three{
            background-color: gold;
            transform: translate(70px,50px);  //원래 위치에서 가로70, 세로50 이동
        }
</style>
```

![](https://i.imgur.com/n3V3TuV.png)

