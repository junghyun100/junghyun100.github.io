---
layout: post
title: "JSP Redirect"
tags: [JSP]
comments: true
---
 
해당 Post는 JSP에서의 Redirect에 대해 정리한 파일입니다.

---

### Redirect

리다이렉트는 HTTP프로토콜로 정해진 규칙입니다.

서버는 클라이언트의 요청에 대해 특정 URL로 이동을 요청할 수 있습니다. 

<strong>이를 리다이렉트라고 합니다.</strong>

서버는 클라이언트에게 HTTP 상태코드 <strong>302</strong>로 응답하는데 이때 헤더 내 Location 값에 이동할 URL 을 추가합니다.

클라이언트는 리다이렉션 응답을 받게 되면 헤더(Location)에 포함된 URL로 재요청을 보내게 됩니다.

이때 브라우저의 주소창은 새 URL로 바뀌게 됩니다.

클라이언트는 서버로부터 받은 상태 값이 302이면 Location헤더값으로 재요청을 보내게 됩니다. 

이때 브라우저의 주소창은 전송받은 URL로 바뀌게 되고

서블릿이나 JSP는 리다이렉트하기 위해 HttpServletResponse 클래스의 sendRedirect() 메소드를 사용합니다.


#### For example

#### redirect01.jsp

```java

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<%
	response.sendRedirect("redirect02.jsp");
%>

```

#### redirect02.jsp

```java

<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	redirect된 페이지입니다.
</body>
</html>

```

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/redirect.PNG" alt="My Image">

해당소스코드를 Run on Server했을 때 나타는 모습이고 redirect01에서 상태값 302를 확인하고 redirect02로 이동한 모습이다.


### 조금 더 알아보기..

JSP/Servlet에서는 현재 작업중인 페이지에서 다른 페이지로 이동하는데 두 가지 방식을 가지고 있습니다

Forwarding과 Redirect로 둘 다 다른 웹 페이지로 이동하지만 처리 형태가 다릅니다.

그중 Forwarding에 대해 알아봅시다.

Web Container 차원에서 페이지 이동만 있습니다.

실제로 웹 브라우저는 다른 페이지로 이동했음을 알 수 없고, 

그렇기 때문에, 웹 브라우저에는 최초에 호출한 URL이 표시되고 이동한 페이지의 URL 정보는 볼 수 없습니다. 

동일한 웹 컨테이너에 있는 페이지로만 이동할 수 있습니다. 

현재 실행중인 페이지와 Forwad에 의해 호출될 페이지는 request와 response 객체를 공유합니다.


---
