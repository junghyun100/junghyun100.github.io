---
layout: post
title: "JSP Forward"
tags: [JSP]
comments: true
---
 
해당 Post는 JSP에서의 Forward에 대해 정리한 파일입니다.

---

3일에 커밋했던 내용에 대해서 보충 내용입니다.

### Forward

1.웹 브라우저에서 Servlet1에게 요청을 보냅니다.

2.Servlet1은 요청을 처리한 후, 그 결과를 HttpServletRequest에 저장

3.Servlet1은 결과가 저장된 HttpServletRequest와 응답을 위한 HttpServletResponse를<br>
같은 웹 어플리케이션 안에 있는 Servlet2에게 전송(forward)

4.Servlet2는 Servlet1으로 부터 받은 HttpServletRequest와 HttpServletResponse를 이용하여 요청을 처리한 후 웹 브라우저에게 결과를 전송

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/Forward.PNG" alt="My Image">

#### For example

#### FrontServlet.java

```java

package example;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class FrontServelt
 */
@WebServlet("/front")
public class FrontServelt extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public FrontServelt() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		int diceValue =(int)(Math.random()*6)+1;
		request.setAttribute("dice", diceValue); //세탁소에 옷을 맡기기 위해서는 주소와 옷을 맡겨야한다.
												 //dice라는 주소를 대고 diceValue라는 옷을 맡겨 두었다라고 생각하라.
		RequestDispatcher requestDispatcher = request.getRequestDispatcher("/next"); //어디로 forward 할 것이냐?
												//같은 웹 어플리케이션에서만 가능, 다른 어플리케이션은 불가능
		requestDispatcher.forward(request, response);//forward 실시, request와 response 넘겨줌
	}

}

```

#### NextServlet.java

```java

package example;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class NextServlet
 */
@WebServlet("/next")
public class NextServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public NextServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#service(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		out.print("<html>");
		out.print("<head><title>form</title></head>");
		out.print("<body>");
		
		int dice = (Integer)request.getAttribute("dice");//request에 있는 값을 읽어들인 값은 오브젝트 값이기 때문에 변수형을 정해줘서 저장함
		out.print("dice : "+dice);
		for(int i = 0; i<dice ; i++)
		{
			out.print("<br>hello");
		}
		out.print("</body>");
		out.print("</html>");
	}

}


```

해당 소스코드를 실행하면 임의의 숫자 random 값을 읽어들여서 그 수만큼 Hello를 반복 출력하는 결과가 나옵니다.

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/Forward2.PNG" alt="My Image">

해당소스코드를 Run on Server했을 때 나타는 모습이고 redirect와는 다르게 URL이 변경되지 않습니다.


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
