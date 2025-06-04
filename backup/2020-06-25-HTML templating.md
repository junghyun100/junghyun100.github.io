---
layout: post
title: "HTML templating"
tags: [WEB UI]
comments: true
---
 
해당 Post는 HTML templating에 대해서 정리한 파일입니다.

서버로부터 받은 데이터를 화면에 반영해야 하는 경우가 많이 있습니다.

그런데 HTML 형태는 그대로이고, 데이터만 변경이 되는 경우가 있을 텐데요.

이럴 때는 어떻게 처리하는 게 효율적인지 알아봅니다.

---

# HTML Templating 이란?

아래 화면에 데이터를 Ajax로 받아와서 화면에 추가해야 한다고 생각해봅니다.

JSON 형태의 데이터를 받을 것이고요.

아래 리스트들의 내용은 모두 다 비슷합니다.

list 태그로 html을 구현해보면 사진, 가격, 이름, 별점, 추가정보(있거나 없거나)를 비슷한 tag를 사용해서 표현하면 됩니다.

여기서 templating 이라는 개념을 도입하면 좋습니다.

<img src="https://cphinf.pstatic.net/mooc/20180207_87/1517990419475nT06l_PNG/3-5-4-1_product_list.png">

## HTML Templating 작업이란?

반복적인 HTML부분을 template로 만들어두고, 서버에서 온 데이터(주로JSON)을 결합해서, 화면에 추가하는 작업이라고 할 수 있습니다.

아래 그림이 이해가 될 겁니다.

<img src="https://cphinf.pstatic.net/mooc/20180207_165/1517990489362QgF8S_PNG/3-5-4-2_about_templating.png">

## 결합과정 해결하기

이제 HTML template과 JSON을 결합하면 됩니다.

간단히 이렇게 구현할 수 있습니다. 

### for example
```js
var data = {  title : "hello",
              content : "lorem dkfief",
              price : 2000
           };
var html = "<li><h4>{title}</h4><p>{content}</p><div>{price}</div></li>";

html.replace("{title}", data.title)
    .replace("{content}", data.content)
    .replace("{price}", data.price)
```

replace는 메서드 체이닝 방식으로 호출하면서 풀이를 할 수 있습니다. 

> 메서드 체인(Method Chaining)이란?<br> 메서드가 객체를 반환하게 되면, 메서드의 반환 값인 객체를 통해 또 다른 함수를 호출할 수 있습니다. 이러한 프로그래밍 패턴을 메서드 체이닝(Method Chaining) 이라 부릅니다

### 생각해보기

data가 배열형태로 여러개가 있다면 어떻게 처리할까요?
간단히 반복문을 쓸 수도 있고, forEach와 같은 메서드를 사용할 수도 있을 겁니다.
위 예제에서 다뤘던 data를 여러개 선언하고, HTML Templating작업을 실습해봅시다.

```js
var data =[
    {
        title : "hello",
        content : "lorem oka",
        price : 2000
    },
    {
        title : "bye",
        content : "lorem oka",
        price : 2000
    },
    {
        title : "goodnight",
        content : "lorem oka",
        price : 2000
    }
];
var html="<li><h4>{title}</h4><p>{content}</p><div>{price}</div></li>";
var resultHtml =[];
data.forEach(function(c) {
    var result = html.replace("{title}", c.title)
                     .replace("{content}", c.content)
                     .replace("{price}", c.price);
    resultHtml.push(result);
    
});

console.log(resultHtml);
```

---
