## 1. Alert박스 UI 디자인

Alert박스를 js로 짜기 전에 HTML과 CSS먼저 짜야합니다.

HTML파일에는 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
  <div class="alert-box">알림창임</div>
  <script>
    
  </script>
</body>
</html>
```

이정도 기초만 잡아놓고

css 파일에는

```css
.alert-box {
    background-color: skyblue;
    padding: 20px;
    color: white;
    border-radius: 5px;
    display: none;
  } 
```

이렇게 alert box  디자인을 간단하게 했습니다.

실행해보면,

![Untitled](https://github.com/map12345678/TIL/assets/158432938/b02599ca-4014-4469-ac2e-7367815b2b0b)

그냥 텍스트만 뜨고 아무런 일도 일어나지 않습니다.

이렇게 디자인을 만들고 js로 기능을 추가하겠습니다.

## 2. 버튼을 누르면 Alert UI 보여주기

짜기 전에 한 가지 더 추가해서 버튼을 누르면 UI를 보여주는 식으로 짜보겠습니다.

`<button onclick=""> 버튼 </button>`

먼저 이런 태그를 써야 하는데 저 큰 따옴표 안에 어떤 js 태그를 넣어야 하냐면

Alert 박스의 아이디의 스타일을 display