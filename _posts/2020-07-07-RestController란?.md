---
layout: post
title: "RestController란?"
tags: [Controller]
comments: true
---
 
해당 Post는 Rest API를 Spring MVC를 이용해 작성하려면 어떻게 해야 하는지 방법에 대해서 정리한 파일입니다.

---

Rest API를 작성하기 위해서 Spring MVC는 @RestController를 제공합니다.

참고로 <strong>Spring 3</strong>에서는 @Controller와 @ResponseBody를 사용했으며,

<strong>Spring 4</strong>에서는 Rest API 또는 Web API를 개발하기 위해서 @RestController이 추가된 애노테이션합니다.

RestController를 사용하기 위해서는 MessageConvertor가 굉장히 중요한데요.

> for example

외부에서 전달받은 JSON 메서드를 내부에서 사용할 수 있는 객체로 변환하거나 컨트롤러를 리턴 한 객체가 <br>
클라이언트에게 JSON으로 변환해서 전달할 수 있도록 하는 역할을 하는 것이 MessageConvertor가 수행을 해줍니다.

이런 MessageConvertor를 @EnableWebMvc로 사용하게 되면 기본으로 제공이 됩니다.

### MessageConvertor

* 자바 객체와 HTTP 요청 / 응답 바디를 변환하는 역할
* @ResponseBody, @RequestBody
* @EnableWebMvc 로 인한 기본 설정
* WebMvcConfigurationSupport 를 사용하여 Spring MVC 구현
* Default MessageConvertor 를 제공

<img src ="https://cphinf.pstatic.net/mooc/20180219_44/1519025088215YszqO_PNG/1.png">

이렇게 보여지는 것처럼 굉장히 다양한 Convertor들이 기본으로 제공이 합니다.

### 그런데 문제점

json으로 변환을 하기 위해서 기본 MessageConvertor는 jackson 라이브러리를 사용하고 있습니다.

#### 만약에 jackson 라이브러리가 제대로 추가되어 있지 않으면??

JSON을 처리하는 Convertor가 등장하지 않기 때문에 500번 오류를 발생시키게 됩니다.

그렇기 때문에 반드시 jackson 라이브러리를 추가해주셔야지만 제대로 사용을 하실 수가 있을 거예요.

사용자가 임의의 메시지 컨버터(MessageConverter)를 사용하도록 하려면 WebMvcConfigurerAdapter의 configureMessageConverters메소드를 오버라이딩 하도록 합니다.

### 생각해보기

Web API에서 JSON메시지를 자주 사용하는 이유는 무엇일까요?

JSON은 기존 XML과 다르게 객체형태와 매우 닮아있어 가독성이 매우 뛰어나고 이로인해 당연히 작성하기도 쉽습니다.

특히 언어나 플랫폼에 종속되지 않기 때문에, 하나의 폼으로 백에서 프론트까지 쉽게 오갈 수 있어 개발이 용이해진다고 생각합니다.

---
