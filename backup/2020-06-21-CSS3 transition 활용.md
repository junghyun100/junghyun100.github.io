---
layout: post
title: "CSS3 transition 활용"
tags: [웹 애니메이션]
comments: true
---
 
해당 Post는 CSS3 transition 활용을 정리한 파일입니다.

애니메이션 효과는 부드러운 UX를 제공하는 것이 좋습니다.

버벅거리는 효과는 오히려 답답하고 느린 웹사이트로 인식될 수 있습니다.

최적의 방법을 찾아서 적용해야 할 텐데요.

이번 장에서 다루는 CSS3 효과가 그 답이라고 해도 될 것 같습니다.

---

## CSS 기법으로 애니메이션 구현

transition 을 이용한 방법입니다. 

이 방법이 JavaScript로 구현하는 것보다 더 빠르다고 알려져 있다.  

특히 모바일 웹에서는 transform을 사용한 element의 조작을 많이 구현합니다.

### CSS 전환

CSS transition부터 시작하겠습니다. 전환은 CSS 변환 휠의 그리스입니다.

transition이 없으면 변환되는 요소가 한 상태에서 다른 상태로 갑자기 변경됩니다.

transition을 적용하여 변경을 제어하여 부드럽고 점진적으로 만들 수 있습니다.

> for example
```html
<div class="wrap">
  <div class="container">
    <h1>Without transition</p>
    <div class="box1 box"></div>
  </div>
   <div class="container">
    <h1>With transition</p>
    <div class="box2 box"></div>
  </div>
</div>
```
```css
.wrap {
  margin: 50px;
}

.container {
  display: inline-block;
  width: 300px;
}

h1 {
  color: lightgray;
  font-family: lato;
  font-size: 20px;
  font-weight: 200;
  padding: 20px;
  text-align: center;
  text-transform: uppercase;
}

.box {
  border-radius: 5px;
  height: 40px;
  margin: 50px auto;
  width: 80px;
  
  .wrap:hover & {
    transform: scale(2);
  }
}

.box1 {
  background: mediumturquoise;
}

.box2 {
  background: #2b3f53;
  transition: all 1s;
}

```

이 포스트에서는 변환과 함께 전환을 사용합니다. 

그러나 요소가 한 스타일에서 다른 스타일로 변경되는 위치 (예 : 마우스를 올리면 버튼의 색상이 변경되는 경우)에서도 전환을 사용할 수 있습니다.

전환이 적용 되려면 두 가지 속성이 필요합니다.

#### 1. transition-property
#### 2. transition-duration

### transition-property (필수)

transition-property전환이 적용될 CSS 속성을 지정합니다. 

개별 속성 (예 : background-color또는 tranform) 또는 규칙 집합의 모든 속성 (예 :  all)에 전환을 적용 할 수 있습니다.

### transition-duration (필수)

transition-duration속성은 전환 시간 범위를 지정합니다. 초 또는 밀리 초 단위로 지정할 수 있습니다.

> for example

```html
<div class="wrap">
  <div class="container">
    <h1>300ms</p>
    <div class="box1 box"></div>
  </div>
   <div class="container">
    <h1>1s</p>
    <div class="box2 box"></div>
  </div>
   <div class="container">
    <h1>3s</p>
    <div class="box3 box"></div>
  </div>
</div>
```
```css
.wrap {
  margin: 50px;
}

.container {
  display: inline-block;
  width: 150px;
}

h1 {
  color: lightgray;
  font-family: lato;
  font-size: 20px;
  font-weight: 200;
  padding: 20px;
  text-align: center;
}

.box {
  border-radius: 50%;
  height: 40px;
  margin: 50px auto;
  width: 40px;
  
  .wrap:hover & {
    transform: scale(2);
  }
}

.box1 {
  background: #60D4C8;
  transition: all 300ms;
}

.box2 {
  background: #46BAAF;
  transition: all 1s;
}

.box3 {
  background: #3EA69B;
  transition: all 3s;
}

```


## 2. 더 빠른 css3 애니메이션 관련 속성들

GPU가속을 이용하는 속성을 사용하면 애니메이션 처리가 빠릅니다.

* transform : translateXX();
* transform : scale();
* transform : rotate();
* opacity

아래 링크를 참고해보세요.

<a href= "https://d2.naver.com/helloworld/2061385">링크 바로가기</a>

---
