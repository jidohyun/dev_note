--- 

### 기본 문법

미디어 쿼리는 css의 어떤 스타일을 선택적으로 적용하고 싶을 때 사용한다.
===if랑 비슷함===

```css
@media (조건) {
  스타일
}
```

스타일 부분에는 조건이 만족했을 때 적용되는 스타일이 들어간다.

### 좁은 스타일링

핸드폰처럼 좁은 화면에서만 특정 스타일을 적용하고 싶을 때는 화면의 최대 너비를 조건으로 하여 미디어 쿼리를 사용할 수 있다.

ex) 800px 이하의 좁은 화면에서 특정 HTML 요소의 배경색을 토마토 색으로 바꾸고 싶다면, 

```html
<div class="small-tomato">좁은 화면에서는 배경색이 토마토 색이 됩니다.</div>
```

```css
@media (max-width: 800px) {
  .small-tomato {
    background-color: tomato;
  }
}
```

이런 식으로 써주면 가로 800px 이하의 화면에선 배경이 토마토가 된다.

### 넓은 화면 스타일링

반대로 넓은 화면에만 특정 스타일을 적용하고 싶다면 화면의 최소 너비를 조건으로 하여 미디어 쿼리를 사용할 수 있음.

ex) 800px 이상의 넓은 화면에서 특정 HTML 요소의 글자색을 토마토 색으로 바꾸고 싶다면, 

```html
<div class="large-tomato">넓은 화면에서는 글자색이 토마토 색이 됩니다.</div>
```

```css
@media (min-width: 800px) {
  .large-tomato {
    color: tomato;
  }
}
```

이런 식으로 써주면 가로 800px 이상의 화면에서 글씨가 토마토가 된다.

### 화면 너비에 따라 HTML 요소 숨기기

미디어 쿼리는 특정 HTML 요소를 화면의 너비에 따라 숨길 때도 많이 사용됨

ex) 목록에서 어떤 항목은 넓은 화면에서 숨기고 어떤 항목은 좁은 화면에서 숨기고 싶다면,

```html
<ul>
  <li class="desktop">저는 넓은 화면에서만 보입니다.</li>
  <li>저는 항상 보입니다.</li>
  <li class="mobile">저는 좁은 화면에서만 보입니다.</li>
</ul>
```

```css
@media (max-width: 600px) {
  .desktop {
    display: none;
  }
}

@media (min-width: 1000px) {
  .mobile {
    display: none;
  }
}
```

600px 이하의 뷰포트라면 desktop 클래스를 가진 태그는 안 보인다.
1000px 이상의 뷰포트라면 mobile 클래스를 가진 태그는 안 보인다.

