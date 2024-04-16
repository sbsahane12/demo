Slip 22: Java Menu Driven Program for Employee Table Operations and JSP Program to Greet User According to Server Time

1. Java menu-driven program for Employee table operations:

This Java program presents a menu-driven interface for performing operations on an Employee table. The assumed Employee table has attributes (ENo, EName, Salary).

```java
import java.util.*;
import java.sql.*;

public class EmployeeMenu {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String url = "jdbc:postgresql://localhost:5432/your_database";
        String username = "username";
        String password = "password";

        try {
            Connection con = DriverManager.getConnection(url, username, password);
            Statement stmt = con.createStatement();
            
            while (true) {
                System.out.println("Menu:");
                System.out.println("1. Insert");
                System.out.println("2. Update");
                System.out.println("3. Display");
                System.out.println("4. Exit");
                System.out.print("Enter your choice: ");
                int choice = scanner.nextInt();

                switch (choice) {
                    case 1:
                        // Insert operation
                        System.out.println("Enter Employee details:");
                        System.out.print("ENo: ");
                        int eno = scanner.nextInt();
                        System.out.print("EName: ");
                        String ename = scanner.next();
                        System.out.print("Salary: ");
                        double salary = scanner.nextDouble();
                        String insertQuery = "INSERT INTO Employee (ENo, EName, Salary) VALUES (" + eno + ", '" + ename + "', " + salary + ")";
                        int insertResult = stmt.executeUpdate(insertQuery);
                        if (insertResult > 0) {
                            System.out.println("Employee inserted successfully.");
                        } else {
                            System.out.println("Error in inserting employee.");
                        }
                        break;

                    case 2:
                        // Update operation
                        // Your code for update operation
                        break;

                    case 3:
                        // Display operation
                        ResultSet rs = stmt.executeQuery("SELECT * FROM Employee");
                        System.out.println("Employee Details:");
                        while (rs.next()) {
                            System.out.println("ENo: " + rs.getInt("ENo") + ", EName: " + rs.getString("EName") + ", Salary: " + rs.getDouble("Salary"));
                        }
                        rs.close();
                        break;

                    case 4:
                        // Exit
                        con.close();
                        System.exit(0);

                    default:
                        System.out.println("Invalid choice. Please enter a valid choice.");
                }
            }
        } catch (Exception e) {
            System.out.println(e);
        } finally {
            scanner.close();
        }
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
    <title>Greeting</title>
</head>
<body>
    <%
        String userName = request.getParameter("userName");
        String greeting = "";
        int hour = java.time.LocalTime.now().getHour();
        if (hour < 12) {
            greeting = "Good morning";
        } else if (hour < 18) {
            greeting = "Good afternoon";
        } else {
            greeting = "Good evening";
        }
    %>
    <h2><%= greeting %>, <%= userName %>!</h2>
    <form action="Greeting.jsp" method="post">
        Enter your name: <input type="text" name="userName">
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```
