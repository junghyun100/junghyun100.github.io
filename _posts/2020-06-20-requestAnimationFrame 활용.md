---
layout: post
title: "requestAnimationFrame 활용"
tags: [웹 애니메이션]
comments: true
---
 
해당 Post는 requestAnimationFrame 활용을 정리한 파일입니다.

---

setTimeout이나 setInterval을 사용해서 연속적인 함수 호출로 애니메이션을 구현하는 방법은 약간의 delay가 발생하는 문제가 있다.

이 함수들은 애니메이션을 위해 생겨난 기능은 아니다. 

애니메이션 구현을 위해서는 끊임없이 부드럽게 처리가 돼야하는데, 다행히도 이를 위한 메서드를 브라우저가 제공하고 있다.

## requestAnimaionFrame

- setTimeout은 animation을 위한 최적화된 기능이라고 보기는 어렵다. 

animation 주기를 16.6 미만으로 하는 경우 불필요한 frame 생성이 되는 등의 문제가 생긴다. 

그 대안으로 생긴 것이 바로 애니메이션을 위한 전용 함수, requestAnimationFrame이다.

- 먼저 아래처럼 requestAnimationFrame을 한번 실행시켜준 후, 

특정 조건이 될 때까지(함수의 탈출 조건) 계속 함수를 연속적으로 호출하는 것이다.

이렇게 requestAnimationFrame 함수를 통해 원하는 함수를 인자로 넣어주면, 

브라우저는 인자로 받은 그 비동기 함수가 실행될 가장 적절한 타이밍에 실행시켜준다.
```js
<!-- html 코드 -->
<html>
    <header>
        <style>
            .outside {
                position: realative;
                background-color: blueviloet;
                width: 100px;
                font-size: 0.8em;
                color: #fff;
            }
        </style>
    </header>
    <body>
        <div class="outside">제가 좋아하는 과일은요..</div>
    </body>
</html>
```
```js
//requestAnimationFrame()
var count = 0;
var el = document.querySelector(".outside");
el.style.left = "0px";
 
function run() {
   if (count > 70) return;
   count = count + 1;
   el.style.left =  parseInt(el.style.left) + count + "px";
   requestAnimationFrame(run);
}
 
requestAnimationFrame(run);
```
예제에서는 연속적으로 requestAnimationFrame을 통해서 run 함수를 호출하면서 left 값을 증가시켜주고 있다.

(횟수로 70회까지 테스트하는 코드이다.) 

canvas, svg를 사용하면서 그래픽 작업을 하게 될 때 복잡한 애니메이션을 다룰 필요가 생길 수 있다.

JavaScript로 복잡한 애니메이션을 처리해야 할 때 requestAnimationFrame은 꽤 유용하게 쓰일 수 있다.

---
