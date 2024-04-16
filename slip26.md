Slip 26: Java Program to Delete Employee Details and JSP Program to Calculate Sum of Digits

1. Java program to delete the details of a given employee (ENo, EName, Salary) by accepting the employee ID through the command line. (Using PreparedStatement Interface)

This Java program deletes the details of a given employee by accepting the employee ID through the command line. It uses PreparedStatement to execute the delete query.

```java
import java.sql.*;

public class DeleteEmployee {
    public static void main(String[] args) {
        if (args.length != 1) {
            System.out.println("Usage: java DeleteEmployee <employee_id>");
            return;
        }

        int empId = Integer.parseInt(args[0]);
        String url = "jdbc:mysql://localhost:3306/your_database";
        String username = "username";
        String password = "password";

        try {
            Connection con = DriverManager.getConnection(url, username, password);
            PreparedStatement pstmt = con.prepareStatement("DELETE FROM Employee WHERE ENo = ?");
            pstmt.setInt(1, empId);

            int rowsAffected = pstmt.executeUpdate();
            if (rowsAffected > 0) {
                System.out.println("Employee details deleted successfully.");
            } else {
                System.out.println("No employee found with the given ID.");
            }

            con.close();
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```
Q2 : 
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Sum of Digits</title>
</head>
<body>
    <%
        String numberStr = request.getParameter("number");
        int number = Integer.parseInt(numberStr);
        int sum = 0;
        int lastDigit = number % 10;
        int firstDigit = 0;

        while (number > 0) {
            firstDigit = number % 10;
            number /= 10;
        }

        sum = firstDigit + lastDigit;
    %>

    <h2 style="color:red; font-size:18px;">Sum of First and Last Digit:</h2>
    <p style="color:red; font-size:18px;"><%= sum %></p>

    <form action="SumOfDigits.jsp" method="post">
        Enter a number: <input type="text" name="number">
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```
