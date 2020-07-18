---
layout: post
title: "handlebar를 활용한 템플릿 작업"
tags: [라이브러리 활용과 클린코드]
comments: true
---
 
해당 Post는 handlebar를 활용한 템플릿 작업에 대해서 정리한 파일입니다.

templating작업은 ES2015에서 template literal로 좀 더 간단해지긴 했습니다.

하지만 여전히 다양한 조건 상황에서의 처리 등은 여전히 복잡합니다.

templating 작업을 돕는 라이브러리는 꽤 다양하고 막강한 방법을 제공합니다.

이를 사용해보면서 templating 처리의 세련된 방법을 알아두면 좋습니다.

---

## 기본 예제

html에 다음과 같이 template코드를 만듭니다.

```js
<script type="myTemplate" id="listTemplate">
	<li>
     <div>게시자 : {{name}}</div>
     <div class="content">{{content}}</div>
     <div>좋아요 갯수 <span> {{like}} </span></div>
     <div class="comment">
       <div>{{comment}}</div>
     </div>
  </li>
</script>	
```
javascript에서는 replace로 치환하는 방법 말고, 라이브러리를 사용해서 결과를 얻을 수 있습니다.
```js
var template = document.querySelector("#listTemplate").innerText;
var bindTemplate = Handlebars.compile(template);  //bindTemplate은 메서드입니다.
```
이제 데이터를 추가합니다.
```js
var data = {
  	"id" : 104,
    "name" : "junghyun",
    "content" : "새로운글을 올렸어요",
    "like" : 5, 
    "comment" : "댓글이다"
};

bindTemplate(data);
```

## 배열이 포함된 데이터의 처리

예를 들어 comment 1개 이상일 수 있습니다.

```js
var data = {
  	"id" : 104,
    "name" : "junghyun",
    "content" : "새로운글을 올렸어요",
    "like" : 5, 
    "comment" : ["댓글이다", "멋진글이네요", "잘봤습니다"]
};

bindTemplate(data);
```
이런 경우는 이렇게 반복문을 넣을 수도 있습니다.

each 부분을 눈여겨봅니다.

```js
<script type="myTemplate" id="listTemplate">
    <li>
        <div>게시자 : {{name}}</div>
        <div class="content">{{content}}</div>
        <div>좋아요 갯수 <span> {{like}} </span></div>
        <div class="comment">
        <h3>댓글목록</h3>
        {{#each comment}}
            <div>{{@index}}번째 댓글 : {{this}}</div>
        {{/each}}
        </div>
    </li>
</script>	
```
compile 부분은 동일하므로 결과를 화면에서 확인합니다.

## data자체가 많아진 경우의 처리

그런데, 실제 데이터는 좀 더 많을 것입니다.

```js
var data = [
	{"id" : 104, "name" : "junghyun", "content" : "새로운글을 올렸어요", "like" : 5, "comment" : ["댓글이다", "잘했어요"]},
	{"id" : 28, "name" : "hary", "content" : "전 오늘도 노래를 불렀어요", "like" : 0, "comment" : ["제발고만..","듣고싶네요 그노래"]},
	{"id" : 23, "name" : "pororo", "content" : "크롱이 항상 말썽을 피워서 행복해~", "like" : 4, "comment" : []},
	{"id" : 5, "name" : "pobi", "content" : "물고기를 한마리도 잡지 못하다니..", "like" : 5, "comment" : ["댓글이다", "멋진글이네요", "잘봤습니다"]}
];
```
반복적으로 결과를 forEach 또는 reduce를 사용해서 합칠 수 있습니다.

### forEach
```js
var innerHtml = "";

data.forEach(function (item, index) {
	innerHtml += bindTemplate(item);
});
```
### reduce
```js
var innerHtml = data.reduce(function(prve, next) {
	return prve + bindTemplate(next);
}, "");
```
## 조건 상황에 따른 처리

지금 예제에서 댓글이 없는 경우에는 다른 메시지를 처리해야 한다면? 

template은 말 그대로 고정되어 있는 것이죠.

하지만 handlebar에서는 이를 지원합니다.

```js
<script type="myTemplate" id="listTemplate">
    <li>
        <div>게시자 : {{name}}</div>
        <div class="content">{{content}}</div>
        <div>좋아요 갯수 <span> {{like}} </span></div>
        <div class="comment">
        <h3>댓글목록</h3>
        {{#if comment}}
            {{#each comment}}
                <div>{{@index}}번째 댓글 : {{this}}</div>
            {{/each}}
        {{else}}
            <div>댓글이 아직 없군요</div>
        {{/if}}
        </div>
    </li>
</script>
```


---
