## 1. js를 쓰는 이유

html 파일 안에 몰래 숨어서 **"html 조작과 변경"** 을 담당하는 언어입니다.

그래서 자바스크립트 코드를 잘 짜시면 html을 원하는대로 마구마구 조작이 가능합니다.

왜 조작을 하냐고요?

- 탭, 모달 등 웹페이지 UI 만들 수 있다.
- 유저가 입력한 데이터를 검사할 수도 있다.
- 유저가 버튼누르면 서버로 데이터 요청할 수도 있다.

이런 기능들을 개발할 수 있습니다.

## 2. js에서 HTML조작 변경하는 방법

먼저 간단한 HTML의 문장을 js로 바꿔보겠습니다.

```html
<h1 id="hello">안녕하세요</h1>

<script>
  document.getElementById('hello').innerHTML = '안녕';
</script>
```

위의 코드를 해석해보자면

**document** **->** 문서인데 여기선 html 웹문서입니다

**마침표 ->** ~의

**getElementById('어쩌구')** **->** 아이디가 '어쩌구'인 html 요소 (일명 element) 를 찾으세요

**innerHTML ->** 딱봐도 그냥 내부 HTML이라는 뜻입니다.

**= ->** 등호는 프로그래밍에서 오른쪽에 있는걸 왼쪽에 대입하라는 뜻입니다.

**'안녕'** **->** 안녕이라는 문자 (큰따옴표, 작은따옴표안에 담겨있으면 항상 문자입니다.)

`document.getElementById('hello').innerHTML = '안녕';` 

특히 이 부분이 js 코드입니다, 해석해보면

“웹문서의 id="hello"인거 찾아서 그거의 내부 HTML에 '안녕' 집어넣어라.” 입니다.

간단하게 실행해보면,

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1 id="hello">안녕하세요</h1>

<script>
    
</script>
</body>
</html>
```

![Untitled](https://github.com/map12345678/TIL/assets/158432938/27cd19e2-1634-4b12-98b8-3604a1a482ce)

이렇게 코드를 짜면 <script><script/> 이 부분이 비어있기 때문에 HTML이 바뀌지

않습니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1 id="hello">안녕하세요</h1>

<script>
    document.getElementById('hello').innerHTML = '안녕';
</script>
</body>
</html>
```

이런 식으로 다시 코드를 짜고 웹에서 새로고침을 해주면

![Untitled](https://github.com/map12345678/TIL/assets/158432938/6caedbc2-efa1-4e43-80cb-67f8cfcb4bcf)

이렇게 바뀌는 걸 볼 수 있습니다.

## 3. 응용

위에 배운 것을 바탕으로 응용을 해보겠습니다.

`document.getElementById('???').src = 'profile.jpg';` 

이렇게 원하는 요소에 src="profile.jpg”를 추가할 수 있고

`document.getElementById('???').style.color = 'red';` 

이러면 원하는 요소에 style="color : red” 처럼 css도 바꿀 수 있습니다.