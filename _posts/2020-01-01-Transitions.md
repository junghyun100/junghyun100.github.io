---
layout: post
title: "응용 css 정리"
tags: [css]
comments: true
---
 
해당 Post는 Transition에 대한 내용임

---

## 응용 css 정리
###  Transition

Transitions는 무엇인가?<br>
Transitions -이동/변경- 을 멋지게 보여주는 효과<br>
<br>

#### forexample

``` java
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>transition</title>
    <style>
      .box{
        background-color : green;
        color:white;
        transition : all .5s ease-in-out;// 변경사항을 0.5초 이내에
      } //ease-in-out 이란 천천히 변경되게 설정함
      .box:hover {
        background-color : blue;
        color : black;
      }
      .box:active {
        background-color : red;
        color : white;
      }
    </style>
  </head>
  <body>
    <span class = "box">
      Text
    </span>
  </body>
</html>
```

Transitions이란 이렇게 어떤 state가 바뀔때 적용이 됨.<br>
state들은 hover, active, focus, visited가 있음.<br>
<ul>
<li>hover : 요소 위에  마우스 커서 올려놓을 때 스타일 지정.<br>
<li>active : 요소를  마우스로 클릭하는 순간의 스타일 지정.<br>
<li>focus : 요소에 커서 위치해 선택된 동안의 스타일 지정.<br>
<li>visited : 방문한 경우 요소의 속성을 스타일 지정.<br>
</li>
</ul>

---