Slip 19: Java Program to Store Negative Integers and Servlet Application to Validate User Credentials

1. Java program to accept 'N' integers from a user, store them into a LinkedList Collection, and display only negative integers:

This Java program accepts 'N' integers from a user, stores them into a LinkedList Collection, and displays only the negative integers.

```java
import java.util.*;

public class NegativeIntegers {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the number of integers (N): ");
        int N = scanner.nextInt();

        LinkedList<Integer> numbers = new LinkedList<>();
        System.out.println("Enter " + N + " integers:");
        for (int i = 0; i < N; i++) {
            int num = scanner.nextInt();
            numbers.add(num);
        }

        System.out.println("Negative Integers:");
        for (int num : numbers) {
            if (num < 0) {
                System.out.println(num);
            }
        }

        scanner.close();
    }
}
```
Q2 : 1
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>User Login</title>
</head>
<body>
    <h2>User Login</h2>
    <form action="UserLoginServlet" method="post">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" required><br><br>
        
        <label for="password">Password:</label>
        <input type="password" id="password" name="password" required><br><br>
        
        <input type="submit" value="Login">
    </form>
</body>
</html>

```

Q2 : 2
```import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.sql.*;

public class UserLoginServlet extends HttpServlet {
    public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();

        // Getting parameters from the request
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        // Database connection parameters
        String url = "jdbc:mysql://localhost:3306/your_database";
        String dbUsername = "username";
        String dbPassword = "password";

        try {
            // Establishing database connection
            Class.forName("com.mysql.jdbc.Driver");
            Connection con = DriverManager.getConnection(url, dbUsername, dbPassword);

            // Query to search for username and password in the database
            PreparedStatement pstmt = con.prepareStatement("SELECT * FROM users WHERE username = ? AND password = ?");
            pstmt.setString(1, username);
            pstmt.setString(2, password);
            ResultSet rs = pstmt.executeQuery();

            if (rs.next()) {
                // User found, display success message
                out.println("<h2>Login Successful</h2>");
                out.println("<p>Welcome, " + username + "!</p>");
            } else {
                // User not found, display error message
                out.println("<h2>Login Failed</h2>");
                out.println("<p>Invalid username or password. Please try again.</p>");
            }

            // Closing resources
            rs.close();
            pstmt.close();
            con.close();
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```
