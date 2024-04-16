Slip 15: Java Program to Display Thread Name and Priority and Servlet Program to Count User Visits Using Cookies

1. Java program to display the name and priority of a Thread:

This Java program displays the name and priority of a thread.

```java
public class ThreadInfo {
    public static void main(String[] args) {
        Thread thread = Thread.currentThread();
        System.out.println("Thread Name: " + thread.getName());
        System.out.println("Thread Priority: " + thread.getPriority());
    }
}
```
Q2: 
```import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class VisitCounterServlet extends HttpServlet {
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();

        // Get the value of the cookie named "visitCount"
        Cookie[] cookies = request.getCookies();
        int visitCount = 0;
        if (cookies != null) {
            for (Cookie cookie : cookies) {
                if (cookie.getName().equals("visitCount")) {
                    visitCount = Integer.parseInt(cookie.getValue());
                    break;
                }
            }
        }

        // Increment visit count and set/update the cookie
        visitCount++;
        Cookie visitCookie = new Cookie("visitCount", String.valueOf(visitCount));
        response.addCookie(visitCookie);

        // Display welcome message or visit count
        if (visitCount == 1) {
            out.println("<h2>Welcome! You are visiting this page for the first time.</h2>");
        } else {
            out.println("<h2>You have visited this page " + visitCount + " times.</h2>");
        }
    }
}
```
