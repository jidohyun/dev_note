
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
그리고 박스를 active(클릭) 하고있는 동안, 핑크박스 