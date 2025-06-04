---
layout: post
title: "Spring MVC구성요소"
tags: [Spring MVC]
comments: true
---
 
해당 Post는 Spring MVC구성요소을 정리한 파일입니다.

Spring MVC에서 가장 핵심적인 역할을 수행하는 DispatcherServlet이 어떤 순서로 동작하는지 살펴보도록 하겠습니다.

이를 통해서 Spring MVC에서 사용되는 컴포넌트들에 대해 알아보도록 하겠습니다.

---

## Spring MVC구성요소

<img src="https://cphinf.pstatic.net/mooc/20180219_116/1519003779294ejdEx_PNG/1.png">

Spring MVC는 모델 2 아키텍처로 구성이 되어있다고 했었습니다.

Database라고 되어있는 이 부분을 제외한 파란색 부분들.

이 부분들은 모두 Spring MVC가 제공해주는 것들 입니다.

개발자가 만들어야 하는 부분들은 보라색으로 되어있는 부분 입니다.

녹색으로 되어있는 View는 Spring이 제공하는 부분도 있고

개발자가 만들어야 되는 부분도 같이 존재한다고 생각하시면 됩니다.

### 그림을 봅시다!

클라이언트가 1번 하고 되어있고, 순서대로 따라가시면 되는데

요청을 보내면 보낸 모든 요청을 Dispatcher Servlet이라고 하는 서블릿 클래스가 받습니다.

Dispatcher Servlet은 요청을 처리해줄 컨트롤러와 메서드가 무엇인지 Handler Mapping에게 질문합니다.

그런데 사실은 Hanlder Mapping이 혼자서 알아낼 수는 없습니다.

#### 이 부분은 어떻게 해야할까요?

> 개발하실 때 Spring MVC로 개발을 하게 되면 어떤 요청에 어떤 컨트롤러가 동작할지를 xml 파일이나 java 파일에 어노테이션으로 설정을 하게 됩니다.<br>
이런 정보들을 Spring으로 만들어진 웹 애플리케이션이 실행할 때 Handler Mapping 객체들이 생성이 되면서 관리를 하게 됩니다.<br>
Dispatcher Servlet은 그렇게 Handler Mapping으로부터 지금 들어온 요청에 알맞은 컨트롤러가 무엇인지 해당되는 메서드는 무엇인지에 대한 정보를 알아내게 됩니다.

이제 Handler Adapter에게 실행을 요청합니다.

그때 결정된 컨트롤러와 해당 메서드가 실행이 되게 될 것이고, 그 결과를 Model에 받아서 Dispatcher Servlet에게 전달을 하게 됩니다.

이때 Dispatcher Servlet은 컨트롤러가 리턴한 view name을 알아오게 될 것입니다.

컨트롤러가 리턴한 view name을 가지고 적절한 View Resolver를 통해서 뷰를 출력하게 됩니다.

View Resolver가 어떤 View이다는 정보를 정확하게 알려주게 되면 이 View로 응답을 하게 되는 과정을 거치게 됩니다.

## DispatcherServlet

* 프론트 컨트롤러 (Front Controller)
* 클라이언트의 모든 요청을 받은 후 이를 처리할 핸들러에게 넘기고 핸들러가 처리한 결과를 받아 사용자에게 응답 결과를 보여준다.
* DispathcerServlet은 여러 컴포넌트를 이용해 작업을 처리한다.

<img src= "https://cphinf.pstatic.net/mooc/20180219_281/1519003870301bOehw_PNG/2.png">
그림 - DispatcherServlet 내부 동작흐름

### 요청 선처리 작업시 사용된 컴포넌트

### org.springframework.web.servlet.LocaleResolver
* 지역 정보를 결정해주는 전략 오브젝트이다.
* 디폴트인 AcceptHeaderLocalResolver는 HTTP 헤더의 정보를 보고 지역정보를 설정해준다.
### org.springframework.web.servlet.FlashMapManager
* FlashMap객체를 조회(retrieve) & 저장을 위한 인터페이스
* RedirectAttributes의 addFlashAttribute메소드를 이용해서 저장한다.
* 리다이렉트 후 조회를 하면 바로 정보는 삭제된다.
### org.springframework.web.context.request.RequestContextHolder
* 일반 빈에서 HttpServletRequest, HttpServletResponse, HttpSession 등을 사용할 수 있도록 한다.
* 해당 객체를 일반 빈에서 사용하게 되면, Web에 종속적이 될 수 있다.
### org.springframework.web.multipart.MultipartResolver
* 멀티파트 파일 업로드를 처리하는 전략

<img src="https://cphinf.pstatic.net/mooc/20180219_91/1519003885824QT31y_PNG/3.png">

그림 - DispatcherServlet 내부 동작흐름 상세 - 요청 선처리 작업

### 요청 전달시 사용된 컴포넌트

### org.springframework.web.servlet.HandlerMapping

* HandlerMapping구현체는 어떤 핸들러가 요청을 처리할지에 대한 정보를 알고 있다.
* 디폴트로 설정되는 있는 핸들러매핑은 BeanNameHandlerMapping과 DefaultAnnotationHandlerMapping 2가지가 설정되어 있다.
### org.springframework.web.servlet.HandlerExecutionChain

* HandlerExecutionChain구현체는 실제로 호출된 핸들러에 대한 참조를 가지고 있다.
* 즉, 무엇이 실행되어야 될지 알고 있는 객체라고 말할 수 있으며, 핸들러 실행전과 실행후에 수행될 HandlerInterceptor도 참조하고 있다.
### org.springframework.web.servlet.HandlerAdapter

* 실제 핸들러를 실행하는 역할을 담당한다.
* 핸들러 어댑터는 선택된 핸들러를 실행하는 방법과 응답을 ModelAndView로 변화하는 방법에 대해 알고 있다.
* 디폴트로 설정되어 있는 핸들러어댑터는 HttpRequestHandlerAdapter, SimpleControllerHandlerAdapter, AnnotationMethodHanlderAdapter 3가지이다.
* @RequestMapping과 @Controller 애노테이션을 통해 정의되는 컨트롤러의 경우 DefaultAnnotationHandlerMapping에 의해 핸들러가 결정되고, 그에 대응되는 AnnotationMethodHandlerAdapter에 의해 호출이 일어난다.

<img src="https://cphinf.pstatic.net/mooc/20180219_20/1519003954110F9wyd_PNG/4.png">

---
