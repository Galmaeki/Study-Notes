## Emmet
- IDE에서 제공하는 자동완성 기능

### 문법
- 요소 생성
  - 생성하려는 요소의 이름 입력 후 키보드 tab 을 누르면 자동완성
- 태그엔 미리 정해진 이름이 없어 아무 이름이나 태그 생성 가능
```html
html:5[tab]
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

```html
hello[tab]
```
```html
<hello></hello>
```

### 구조화
- 요소들의 구조를 간편히 생성 가능
#### 자식요소 >
- \>를 사용하여 자식요소 행성 가능
```html
div>ul>li
```
```html
<div>
    <ul>
        <li></li>
    <ul>
<div>
```

#### 형제요소 +
- +를 하용하여 같은단계에 위치한 요소 생성
```html
div+p+bq
```
```html
<div></div>
<p></p>
<blockquote></blockquote>
```

#### 한 단계 올리기 ^
- ^를 사용하여 한 단계 위에 요소 배치 가능
```html
div+div>p>span+em^bq
```
```html
<div></div>
<div>
    <p>
        <span></span>
        <em></em>
    </p>
    <blockquote></blockquote>
</div>
```
- ^를 여러번 사용하면 그만큼 상위에 생성됨
```html
div+div>p>span+em^^bq
```
```html
<div></div>
<div>
  <span>
    <p><span></span><em></em></p>
  </span>
  <blockquote></blockquote>
</div>
```

#### 반복 *
- *로 원하는갯수만큼 추가 가능
```html
ul>li*5
```
```html
<ul>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
</ul>
```