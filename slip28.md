Slip 28: JSP Script to Display String in Reverse Order and Java Program to Display Name of Currently Executing Thread

1. JSP script to accept a String from a user and display it in reverse order:

This JSP script accepts a String from a user and displays it in reverse order.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Reverse String</title>
</head>
<body>
    <h2>Reverse String</h2>
    <form action="ReverseString.jsp" method="post">
        Enter a string: <input type="text" name="inputString">
        <input type="submit" value="Submit">
    </form>

    <%
        String inputString = request.getParameter("inputString");
        if (inputString != null) {
            String reversedString = new StringBuilder(inputString).reverse().toString();
            out.println("<p>Original String: " + inputString + "</p>");
            out.println("<p>Reversed String: " + reversedString + "</p>");
        }
    %>
</body>
</html>

```
Q2:First Way
```

public class CurrentThreadName {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            System.out.println("Currently executing thread: " + Thread.currentThread().getName());
        });
        thread.start();
    }
}

```
Q2:Second Way

```
public class CurrentThreadName {
    public static void main(String[] args) {
        // Create a custom thread
        Thread customThread = new Thread(() -> {
            System.out.println("Custom thread executing: " + Thread.currentThread().getName());
        });

        // Start the custom thread
        customThread.start();

        // Display the name of the main thread
        System.out.println("Main thread executing: " + Thread.currentThread().getName());
    }
}

```
