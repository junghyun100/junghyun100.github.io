---
layout: post
title: "웹 애니메이션 이해와 setTimeout 활용"
tags: [웹 애니메이션]
comments: true
---
 
해당 Post는 웹 애니메이션 이해와 setTimeout 활용을 정리한 파일입니다.

---

## 1. 애니메이션

애니메이션은 반복적인 움직임의 처리입니다.

웹 UI 애니메이션은 자바스크립트로 다양하게 제어할 수 있지만, 규칙적이고 비교적 단순한 방식으로 동작하는 방식은 CSS3의 transition과 transform속성을 사용해서 대부분 구현 가능합니다.

그뿐만 아니라 자바스크립트보다 더 빠른 성능을 보장한다고 합니다.

이런 애니메이션 성능 비교가 된 글을 찾아보고 비교해봅니다.

특히 모바일 웹에서는 CSS를 사용한 방법이 훨씬 더 빠릅니다.

## 2. FPS

FPS (frame per second)는 1초당 화면에 표현할 수 있는 정지화면(frame) 수입니다.

매끄러운 애니메이션은 보통 60fps 입니다.

따라서 16.666밀리세컨드 간격으로 frame 생성이 필요한 셈이죠. (1000ms / 60fps = 16.6666ms)

앞서 말한 것처럼, 애니메이션은 CSS의 transition속성으로 CSS 속성을 변경하거나, JavaScript로 CSS 속성을 변경할 수 있습니다.

결국 이렇게 정의할 수 있습니다.

* 간단하고 규칙적인 거 → CSS로 변경
* 세밀한 조작이 필요한 거 → JavaScript로 변경
* 성능만 봐서는 대체로 CSS로 변경하는 것이 빠릅니다.

하지만 성능 비교를 통해서 가장 빠른 방법을 찾는 과정이 필요하기도 합니다.

## 3. JavaScript 애니메이션
자바스크립트로 애니메이션을 구현하려면, 규칙적인 처리를 하도록 구현하면 된다.

- setInterval

- setTimeout

- requestAnimaitionframe

- Animation API

### 3.1 setInterval()

주어진 시간에 따라서 함수 실행이 가능합니다.
```js
const interval = window.setInterval(()=> {
  console.log('현재시각은', new Date());
},1000/60);
```
window.clearInterval(interval);
하지만 지연문제가 발생할 수 있습니다.

아래 그림을 보면 제때 일어나야 할 이벤트 콜백이 지연되고, 없어지고 하는 것을 볼 수 있습니다.

따라서 setInterval을 사용한다고 해서 정해진 시간에 함수가 실행된다고 보장할 수 없습니다.

<img src = "https://www.edwith.org/viewer/image?src=https%3A%2F%2Fcphinf.pstatic.net%2Fmooc%2F20180205_116%2F1517816517181oVblH_PNG%2F3-4-1_setInterval.png">

setInterval
일반적으로 setInterval 을 사용하는 애니메이션 구현을 잘 하지 않습니다.

### 3.2 setTimeout()
```js
//arrow 함수를 사용했어요.
  setTimeout(() => {
    console.log('현재시각은', new Date());
  },500);
  ```
  
  애니메이션을 구현하려면 재귀호출을 해서 구현할 수 있습니다.
  
  ```js
  let count = 0;
function animate() {   
  setTimeout(() => {
    if(count >= 20) return;
    console.log('현재시각은', new Date());
    count++;
    animate();
  },500);
}
animate();
```

setTimeout도 엄밀하게는 어떤 이유로 인해 제때에 원하는 콜백함수가 실행되지 않을 수는 있습니다. 

결국 setInterval과 같은 문제가 발생할 수도 있긴합니다.  

하지만 그럼에도 setTimeout은 매 순간 timeout을 조절할 수 있는 코드구현으로 컨트롤 가능한 부분이 있다는 점이

setInterval과 큰 차이라고 할 수 있습니다.

---
