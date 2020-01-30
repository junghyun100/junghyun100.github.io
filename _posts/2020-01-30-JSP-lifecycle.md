---
layout: post
title: "JSP-LifeCycle"
tags: [JSP]
comments: true
---
 
해당 Post는 JSP의 LifeCycle를 정리한 파일입니다.

---

#### 들어가기 전

WAS는 웹 브라우저로부터 JSP에 대한 요청을 받게 되면, JSP코드를 서블릿 소스코드로 변환한 후 컴파일 하여 실행되게 됩니다.

서블릿으로 컴파일되어 실행될 때 상황에 따라서 어떤 메소드들이 실행되는지 잘 알아야, JSP를 알맞게 작성할 수 있습니다.

JSP 페이지는 .jsp 확장명으로 저장되어 서버가이 페이지가 JSP 페이지이며 JSP 수명주기 단계를 거쳐야 함을 알려줍니다.


####  JSP의 실행순서

1. 브라우저가 웹서버에 JSP에 대한 요청 정보를 전달한다.

2. 브라우저가 요청한 JSP가 최초로 요청했을 경우만 JSP로 작성된 코드가 서블릿으로 코드로 변환한다. (java 파일 생성)

3. 서블릿 코드를 컴파일해서 실행가능한 bytecode로 변환한다. (class 파일 생성)

4. 서블릿 클래스를 로딩하고 인스턴스를 생성한다.

5-1. jspInit()메소드 호출을 통한 초기화

5-2. _jspService()메소드 호출을 통한 요청 처리

5-3. jspDestroy()메소드 를 호출하여 파기

>서블릿 라이프 싸이클에서 실행되는 메소드의 이름은 init(),  service(), destroy()이고,

>JSP 라이프 싸이클에서 실행되는 메소드의 이름은 jspInit(), _jspService(), jspDestory()입니다.

#### JSP 코드 예시

```java

<html>
    <head>
        <title>My First JSP Page</title>
    </head>
    <%
       int count = 0;
    %>
    <body>
        Page Count is:  
        <% out.println(++count); %>
    </body>
</html>

```
위의 JSP 페이지 (hello.jsp)가 아래의 서블릿이됩니다.

```java

public class hello_jsp extends HttpServlet
{
  public void _jspService(HttpServletRequest request, HttpServletResponse response) 
                               throws IOException,ServletException
   {
      PrintWriter out = response.getWriter();
      response.setContenType("text/html");
      out.write("<html><body>");
      int count=0;
      out.write("Page count is:");
      out.print(++count);
      out.write("</body></html>");

   }
}

```

#### JSP의 특징

---
