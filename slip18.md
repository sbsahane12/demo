Slip 18: Java Program to Display Thread Name and Priority and Servlet Program to Accept Student Details

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
```
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class StudentDetailsServlet extends HttpServlet {
    public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();

        // Getting parameters from the request
        int seatNo = Integer.parseInt(request.getParameter("seatNo"));
        String studentName = request.getParameter("studentName");
        String studentClass = request.getParameter("studentClass");
        int totalMarks = Integer.parseInt(request.getParameter("totalMarks"));

        // Calculating percentage and grade
        double percentage = (double) totalMarks / 500 * 100;
        String grade;
        if (percentage >= 90) {
            grade = "A+";
        } else if (percentage >= 80) {
            grade = "A";
        } else if (percentage >= 70) {
            grade = "B+";
        } else if (percentage >= 60) {
            grade = "B";
        } else if (percentage >= 50) {
            grade = "C";
        } else {
            grade = "F";
        }

        // Displaying student details
        out.println("<h2>Student Details</h2>");
        out.println("<p>Seat No: " + seatNo + "</p>");
        out.println("<p>Student Name: " + studentName + "</p>");
        out.println("<p>Class: " + studentClass + "</p>");
        out.println("<p>Total Marks: " + totalMarks + "</p>");
        out.println("<p>Percentage: " + percentage + "%</p>");
        out.println("<p>Grade: " + grade + "</p>");
    }
}
```
Q2 1:
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Student Details</title>
</head>
<body>
    <h2>Enter Student Details</h2>
    <form action="StudentDetailsServlet" method="post">
        <label for="seatNo">Seat No:</label>
        <input type="text" id="seatNo" name="seatNo" required><br><br>
        
        <label for="studentName">Student Name:</label>
        <input type="text" id="studentName" name="studentName" required><br><br>
        
        <label for="studentClass">Class:</label>
        <input type="text" id="studentClass" name="studentClass" required><br><br>
        
        <label for="totalMarks">Total Marks:</label>
        <input type="text" id="totalMarks" name="totalMarks" required><br><br>
        
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```

