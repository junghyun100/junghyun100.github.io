---
layout: post
title: "JSP"
tags: [JSP]
comments: true
---
 
해당 Post는 JSP와 Servelt연동하는 것에 대해 정리한 파일입니다.

---

### JSP와 Servelt연동

Servlet은 프로그램 로직이 수행되기에 조금 유리한 구조입니다.

서블릿은 자체적으로 자바 파일이니까 프로그램 로직이 수행되는데 JSP보다 조금 더 편합니다.

그리고 JSP는 결과를 출력할 때 서블릿보다 유리합니다

서블릿에서 HTML 페이지를 하나 만들어내려면 out.println에서 문자열로 html 태그들을 다 넣어줬어야하지만

JSP는 그냥 HTML태그만 넣으면 됩니다.

#### 이런 점을 보아 서블릿과 JSP는 각각 이런 장점과 단점을 가지고 있습니다.

이런 서블릿과 JSP의 장단점을 해결하기 위해서 

Servlet에서는 프로그램 로직을 수행하게 하고, 

그 결과를 JSP에서 출력하게 해주는 방식을 이용합니다.

이렇게 수행하는 것을 Servlet과 JSP의 연동이라고 합니다.

### Servlet과 JSP연동 정리.

1)Servlet은 프로그램 로직이 수행되기에 유리하다. IDE 등에서 지원을 좀 더 잘해준다.

2)JSP는 결과를 출력하기에 Servlet보다 유리하다. 필요한 html문을 그냥 입력하면 됨.

3)프로그램 로직 수행은 Servlet에서, 결과 출력은 JSP에서 하는 것이 유리하다.

4)Servlet과 JSP의 장단점을 해결하기 위해서 Servlet에서 프로그램 로직이 수행되고,<br>
그 결과를 JSP에게 포워딩하는 방법이 사용되게 되었다.

#### for Example(LogicServelt.java)

```javascript


import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class LogicServelt
 */
@WebServlet("/logic")
public class LogicServelt extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public LogicServelt() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		int v1 = (int)(Math.random()*100)+1;
		int v2 = (int)(Math.random()*100)+1;
		int result = v1+v2;
		
		request.setAttribute("v1", v1);
		request.setAttribute("v2", v2);
		request.setAttribute("result", result);
		
		RequestDispatcher rd = request.getRequestDispatcher("/result.jsp");
		rd.forward(request, response);
	}

}


```

#### for Example(result.jsp)

```javascript

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<%
	int v1 = (int)request.getAttribute("v1");
	int v2 = (int)request.getAttribute("v2");
	int result = (int)request.getAttribute("result");
%>
<%= v1 %>+ <%=v2 %> = <%= result %>
</body>
</html>
```  

위 과정은 아래의 그림과 같은 로직으로 형성이 됩니다.

<img src="https://cphinf.pstatic.net/mooc/20180129_201/1517203743283AcQbB_PNG/2_4_3_servlet_jsp.PNG">



---
