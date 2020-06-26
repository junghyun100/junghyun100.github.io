---
layout: post
title: "HTML templating 실습"
tags: [WEB UI]
comments: true
---
 
해당 Post는 HTML templating에 대해서 정리한 파일입니다.

HTML Templating작업을 하기 위해서는 Ajax로 데이터를 가져오는 방법도 필요하고, 또 한 가지 template을 어딘가 보관해야 할 겁니다.

몇 가지 방법이 있겠지만, 간단한 방법을 알아볼 예정입니다. 


---

# HTML Template의 보관

HTML Template 보관이라는 게 있습니다.

우리가 템플릿 코드를 어떻게 보관할 것이냐.

어떤 코드가 이제 템플릿이라고 치면

이거 자바스크립트 코드 안에서 보관하는 건?

굉장히 안 좋습니다.

자바스크립트 함수라는 건 뭔가요.

어떤 데이터, 로직을 처리하는 것인데 이렇게 많은 부분을 굉장히 긴 템플릿이다 그러면 자바스크립트 코드를 해석하는 게

굉장히 어려울 것 입니다. 가독성이 떨어지는 코드가 될 것이고.

```js
var html = "<li><h4>{title}</h4><p>{content}</p><div>{price}</div></li>";
```
* 서버에서 file로 보관하고 Ajax로 요청해서 받아옵니다.
* HTML코드 안에 숨겨둔다(?)
간단한 것이라면 HTML 안에 숨겨둘 수가 있습니다.

숨겨야 할 데이터가 많다면 별도 파일로 분리해서 Ajax로 가져오는 방법도 좋습니다.

하지만 많지 않은 데이터이므로 우리는 HTML 안에 잘 보관해두겠습니다.

## Templating

HTML 중 script 태그는 type이 javascript가 아니라면 렌더링하지 않고 무시합니다.

바로 이걸 이용하는 겁니다.

```js
<script id="template-list-item" type="text/template">
  <li>
      <h4>{title}</h4><p>{content}</p><div>{price}</div>
  </li>
</script>
```
이렇게 간단히 javascript에서 가져올 수가 있을 겁니다.

```js
var html = document.querySelector("#template-list-item");
```

이후 작업은 replace로 하면 끝나죠.

```js
var data = [
        {title : "hello",content : "lorem dkfief",price : 2000},
        {title : "hello",content : "lorem dkfief",price : 2000}
];

//html 에 script에서 가져온 html template.
var html = document.querySelector("#template-list-item").innerHTML;

var resultHTML = "";

for(var i=0; i<data.length; i++) {
    resultHTML += html.replace("{title}", data[i].title)
                      .replace("{content}", data[i].content)
                      .replace("{price}", data[i].price);
}

document.querySelector(".content").innerHTML = resultHTML;
```

---
