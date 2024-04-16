Slip 24: Java Program to Scroll Text and JSP Script for User Authentication

1. Java program to scroll text from left to right continuously:

This Java program scrolls text from left to right continuously.

```java
import javax.swing.*;

public class TextScroll extends JFrame implements Runnable {
    private JLabel label;
    private String text;
    private int delay;

    public TextScroll(String text, int delay) {
        this.text = text;
        this.delay = delay;
        label = new JLabel(text, JLabel.LEFT);
        add(label);
        setSize(400, 50);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setVisible(true);

        Thread thread = new Thread(this);
        thread.start();
    }

    public void run() {
        while (true) {
            try {
                text = text.substring(1) + text.charAt(0);
                label.setText(text);
                Thread.sleep(delay);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        new TextScroll("Scrolling Text Demo", 200);
    }
}
```
Q2:
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Login Result</title>
</head>
<body>
    <%
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        if (username.equals(password)) {
            response.sendRedirect("Login.html");
        } else {
            response.sendRedirect("Error.html");
        }
    %>
</body>
</html>

<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Login Successful</title>
</head>
<body>
    <h2>Login Successful</h2>
</body>
</html>

<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Login Failed</title>
</head>
<body>
    <h2>Login Failed</h2>
</body>
</html>

<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Login Form</title>
</head>
<body>
    <h2>Login Form</h2>
    <form action="Authenticate.jsp" method="post">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" required><br><br>
        
        <label for="password">Password:</label>
        <input type="password" id="password" name="password" required><br><br>
        
        <input type="submit" value="Login">
    </form>
</body>
</html>

```
