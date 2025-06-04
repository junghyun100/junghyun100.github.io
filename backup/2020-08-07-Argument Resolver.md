---
layout: post
title: "Argument Resolver란?"
tags: [Argument Resolver]
comments: true
---
 
해당 Post는 Argument Resolver에 대해서 정리한 파일입니다.

---

## Argument Resolver(Interceptor)란?

* 컨트롤러의 메소드의 인자로 사용자가 임의의 값을 전달하는 방법을 제공하고자 할 때 사용됩니다.
* 예를 들어, 세션에 저장되어 있는 값 중 특정 이름의 값을 메소드 인자로 전달합니다.
 
 ## Argument Resolver 작성방법 

#### 1. org.springframework.web.method.support.HandlerMethodArgumentResolver를 구현한 클래스를 작성합니다.
#### 2. supportsParameter메소드를 오버라이딩 한 후, 원하는 타입의 인자가 있는지 검사한 후 있을 경우 true가 리턴되도록 합니다.
#### 3. resolveArgument메소드를 오버라이딩 한 후, 메소드의 인자로 전달할 값을 리턴합니다.

이렇게 작성한 파일을 Java Config에다가 설정을 하는 방법은 WebMvcConfigurerAdapter를 상속받은 Java Config 파일에서

addArgumentResolvers라는 메서드를 오버라이딩 한 후에 원하는 아규먼트 리졸버 클래스 객체를 등록을 하면 됩니다.

xml 파일을 이용해서 설정을 하는 경우에는 어노테이션 기반으로 작성을 합니다.

```js
   <mvc:annotation-driven>
        <mvc:argument-resolvers>
            <bean class="Argument Resolver"></bean>      
        </mvc:argument-resolvers>
    </mvc:annotation-driven>
 ```
엘리먼트 안에다가 Argument 속성들을 넣어주시고 여기에 bean class로 Argument Resolver를 등록하면 사용할 수 있습니다.

Spring MVC가 제공하고 있는 기본 ArgumentResolver들이 이렇게 많이 존재하고 있습니다.

Controller 메서드에 HttpServlet Request나 HttpSession 등을 적으면 값이 전달되는 것을 볼 수 있습니다.

이런 일이 가능한 이유는 <strong>Spring MVC가 기본으로 제공하는 Argument Resolver</strong>가 있기 때문에 그렇습니다.

## Spring MVC의 기본 ArgumentResolver들

getDefaultArgumentResolvers()메소드를 보면 기본으로 설정되는 아규먼트 리졸버에 어떤 것이 있는지 알 수 있습니다.

Map객체나 Map을 상속받은 객체는 Spring에서 이미 선언한 아규먼트 리졸버가 처리하기 때문에 전달 할 수 없습니다.

Map객체를 전달하려면 Map을 필드로 가지고 있는 별도의 객체를 선언한 후 사용해야 합니다.

---
