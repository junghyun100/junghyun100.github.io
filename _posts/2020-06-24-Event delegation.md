---
layout: post
title: "Event delegation"
tags: [WEB UI]
comments: true
---
 
해당 Post는 Event delegation에 대해서 정리한 파일입니다.

list(어떤 목록)가 여러 개인 UI에 각각 비슷한 이벤트를 걸어서 처리해야 한다면 어떻게 해야 할까요? 

for문으로 addEventListener를 사용해야 할까요?

---

### 이런 상황에서의 이벤트 등록

아래 화면은 가로로 배치된 책 리스트입니다.

각각 리스트에 클릭을 할 때 어떤 이벤트가 발생해야 한다고 가정합니다.

addEventListener를 사용해서 이벤트 등록을 할 수 있을겁니다.

<img src="https://www.edwith.org/viewer/image?src=https%3A%2F%2Fcphinf.pstatic.net%2Fmooc%2F20180207_239%2F15179786577601DaLw_PNG%2F3-5-3-image-list.png">

### for example

```html
<ul>
  <li>
    <img src="https://images-na.,,,,,/513hgbYgL._AC_SY400_.jpg" class="product-image" >    </li>
  <li>
    <img src="https://images-n,,,,,/41HoczB2L._AC_SY400_.jpg" class="product-image" >    </li>
  <li>
    <img src="https://images-na.,,,,51AEisFiL._AC_SY400_.jpg" class="product-image" >  </li>
 <li>
    <img src="https://images-na,,,,/51JVpV3ZL._AC_SY400_.jpg" class="product-image" >
 </li>
</ul>
```
li 각각에 addEventListener를 통해 이벤트를 등록합니다. 

이 코드는 잘 동작합니다.

```js
var log = document.querySelector(".log");
var lists = document.querySelectorAll("ul > li");

for(var i=0,len=lists.length; i < len; i++) {
  lists[i].addEventListener("click", function(evt) {
     log.innerHTML = "clicked" + evt.currentTarget.firstChild.src;
  });
}
```

브라우저는 4개의 이벤트 리스너를 기억하고 있습니다.

그런데 list가 훨씬 더 많다면 브라우저는 기억해야 할 이벤트 리스너도 그만큼 많아집니다.

비효율적이죠.  

문제는 한가지 더 있습니다. 만약 list가 한 개 더 동적으로 추가된다면 어떻게 될까요?

네, 추가된 엘리먼트에 역시 addEventListener를 해줘야 합니다.

이것도 꽤 불편한 일 같네요.

target 정보가 우리를 돕습니다.

자, 이번에는 ul 태그에만 이벤트를 새롭게 등록합니다. 

```
ul.addEventListener("click",function(evt) {
    console.log(evt.currentTarget, evt.target);
});
```
이럴 경우 li안에 이미지를 클릭하면 위 결과는 무엇일까요?

만약 ul > li > img 태그를 클릭했다면 어떤 결과가 나올까요?

그 전에 이벤트는 실행은 될까요?

정답은 '네' 입니다. 

 li 나 img 태그는 ul 태그에 속하기도 합니다.

따라서 UL에 등록한 이벤트 리스너도 실행이 됩니다. 

이것은 <strong>이벤트 버블링</strong>이라고 합니다.

> 클릭한 지점이 하위엘리먼트라고 하여도, 그것을 감싸고 있는 상위 엘리먼트까지 올라가면서 이벤트리스너가 있는지 찾는 과정입니다. 

만약 img, li, ul에 각각 이벤트를 등록했었다면, 3개의 이벤트 리스너가 실행했을 겁니다. 

아래 이미지는 하위엘리먼트는 3번부터 이벤트가 발생하고 2,1 순으로 이벤트가 발생했습니다.

비슷하게 Capturing이라는 것도 있습니다. 반대로 이벤트가 발생하는 것인데요.

기본적으로는 Bubbling 순서로 이벤트가 발생합니다.

따라서 Bubbling을 잘 기억해두는 게 좋습니다.

Capturing 단계에서 이벤트 발생을 시키고 싶다면 addEventListener 메서드의 3번째 인자에 값을 true로 주면 됩니다. 

<img src= "https://www.edwith.org/viewer/image?src=https%3A%2F%2Fcphinf.pstatic.net%2Fmooc%2F20180207_43%2F1517986448399nM5Jy_PNG%2F3-5-3_Event_Bubbling.png">

## Event delegation

우리는 img나 li를 클릭해도 UL에도 이벤트가 발생하고, 이벤트 리스너 실행된다는 것을 알게 됐습니다.

img를 클릭하면 아래 결과는 무엇이 나올까요?

ul 그리고 img 태그가 나옵니다. 

```js
ul.addEventListener("click",function(evt) {
    console.log(evt.currentTarget.tagName, evt.target.tagName);
});
```
이제 addEventListener 메서드를 한 번만 쓰면서 우리는 모든 list의 image 정보를 확인할 수 있습니다.

더구나 list 태그가 하나 더 추가된다고 하여도 문제없이 동작합니다.
```js
var ul = document.querySelector("ul");
ul.addEventListener("click",function(evt) {
    if(evt.target.tagName === "IMG") {
      log.innerHTML = "clicked" + evt.target.src;
    }
});
```
그런데 작은 문제가 하나 더 있는 거 같네요.

예제를 보면, 이미지 태그는 padding 값이 있어서, img태그와 li 태그 사이에 공백이 존재합니다.

이 부분(공백)을 클릭하면 tagName이 LI라서 위에서 구현한 조건문으로 들어가지 않았기 때문입니다.

이 부분(공백)을 클릭해도 이미지 url을 출력할 수 있으려면 어떻게 해야할까요?
```js
var ul = document.querySelector("ul");
ul.addEventListener("click",function(evt) {
  debugger;
    if(evt.target.tagName === "IMG") {
      log.innerHTML = "clicked" + evt.target.src;
    } else if (evt.target.tagName === "LI") {
      log.innerHTML = "clicked" + evt.target.firstChild.src;
    }
});
```

### 참고하면 좋을 사이트

<a href="https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/">이벤트 버블링, 이벤트 캡처 그리고 이벤트 위임까지</a>


---
