---
layout: post
title: "Tab UI를 만들기 위한 HTML과 CSS 구조전략"
tags: [Tab UI]
comments: true
---
 
해당 Post는 Tab UI를 만들기 위한 HTML과 CSS 구조전략에 대해서 정리한 파일입니다.

Tab은 자주 사용되는 UI 중 하나입니다.  

많은 컨텐츠를 효과적으로 보여줄 수 있기 때문이겠죠.


---

# Tab UI는 어떤 건가요?

<img src="https://cphinf.pstatic.net/mooc/20180207_26/1517992864436V3FGa_PNG/3-6-1_Tab_UI_image.png">

예시를 보았을 때 이런 여러 가지 메뉴들이 있습니다.

누르게 되면 하단에 있는 내용이 통째로 바뀌는 것이죠.

그러면 하단에 있는 내용은 꼭 이런 형태가 아닐 수 있어요.

위에 탭을 4개 정도 만들고 하단에 텍스트 위주로만 표현하는 거를 어떻게 짜는지 한번 알아보도록 하겠습니다.

## 만들어보자!

여러 가지 방법이 있겠지만 위에 있는 메뉴는 쭉 나열을 하고 아래쪽에는 어떻게 할까요?

아래쪽에는 HTML 하나를 같이 share 하는 식으로 하겠습니다.

```html
<html>
<header>
    <style>
        h2{ 
            text-align: center;
        }
        h2, h4 {
            margin: 0px;
        }
        .tab{
            width: 500px;
            margin: 0px auto;
        }
        .tab-menu{
            background-color: #888888;
        }
        .tab-menu > div{
            display: inline-block;
            width: 120px;
            height: 50px;
            line-height: 50px;
            text-align: center;
            cursor: pointer;
        }
        .content{
            padding: 5%;
            background-color: antiquewhite;
        }
    </style>
</header>

<body>
    <h2>TAB UI TEST</h2>

    <div class="tab">
        <div class="tab-menu">
            <div>Menu1</div>
            <div>Menu2</div>
            <div>Menu3</div>
            <div>Menu4</div>
        </div>
        <section class="content">
            <h4>Hello Sir</h4>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.
                Quibusdam, ipsum.</p>
        </section>
    </div>
</body>

</html>
```

#### tab메뉴의 가로폭을 px로 조정했지만, float와 같은 속성을 사용하는 것이 더 좋습니다.

이런 식으로 한번 구현을 간단하게 해볼 수가 있었습니다.

Tab UI를 통해서 우리가 HTML 구조를 한번 간단하게 짜봤는데

HTML 짤 수 있는 태그들, 위에를 지금은 div를 썼지만 nav라는 태그를 쓸 수도 있습니다.

또는 ul, li을 줘서 위에 메뉴들도 리스트를 표현할 수도 있고

간단하게 inline-block으로

> (inline-block이 약간 예외적으로 동작되는 그런 것들이 있긴 한데 그 특성을 우리가 좀 이해를 못 하더라도<br>
inline-block을 써서 좌우로 배치하는 메뉴 같은 것들은 배치하는 것 손쉽게 할 수가 있습니다.)

중요한 거는 하단의 콘텐츠 본문 영역을 4 개를 두지 않고

물론 4개를 두고 display를 block과 none을 왔다 갔다 하면서

교차하면서 해결할 수도 있는데

콘텐츠 하나를 두고 그 안에 있는 내용들을 이제 계속 바꿔주는 식으로

한번 만들어보는 것도 좋은 전략일 것 같습니다.

---
