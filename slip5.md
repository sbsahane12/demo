Slip 5: Java Program - Hash Table and JSP Page for Online Multiple Choice Test

1. Java Program to create a hash table that will maintain the mobile number and student name. Display the details of the student using the Enumeration interface:

```java
import java.util.Enumeration;
import java.util.Hashtable;

public class StudentDetails {
    public static void main(String[] args) {
        Hashtable<String, String> studentHashTable = new Hashtable<>();

        // Adding student details to the hash table
        studentHashTable.put("1234567890", "John");
        studentHashTable.put("9876543210", "Jane");
        studentHashTable.put("5555555555", "Alice");

        // Displaying student details using Enumeration
        Enumeration<String> mobileNumbers = studentHashTable.keys();
        while (mobileNumbers.hasMoreElements()) {
            String mobileNumber = mobileNumbers.nextElement();
            String studentName = studentHashTable.get(mobileNumber);
            System.out.println("Mobile Number: " + mobileNumber + ", Student Name: " + studentName);
        }
    }
}
JSP page for an online multiple choice test:
jsp
Copy code
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Online Multiple Choice Test</title>
</head>
<body>
    <h2>Online Multiple Choice Test</h2>
    <%
        try {
            // Establishing database connection
            Class.forName("org.postgresql.Driver");
            Connection con = DriverManager.getConnection("jdbc:postgresql://localhost:5432/your_database", "username", "password");
            Statement stmt = con.createStatement();
            ResultSet rs = stmt.executeQuery("SELECT * FROM questions ORDER BY RANDOM() LIMIT 1"); // Randomly selecting a question
            
            if (rs.next()) {
                String question = rs.getString("question");
                String choice1 = rs.getString("choice1");
                String choice2 = rs.getString("choice2");
                String choice3 = rs.getString("choice3");
                String choice4 = rs.getString("choice4");
    %>
    <form method="post" action="submitTest.jsp">
        <p><%= question %></p>
        <input type="radio" name="answer" value="1"> <%= choice1 %><br>
        <input type="radio" name="answer" value="2"> <%= choice2 %><br>
        <input type="radio" name="answer" value="3"> <%= choice3 %><br>
        <input type="radio" name="answer" value="4"> <%= choice4 %><br>
        <input type="submit" value="Next">
    </form>
    <%
            } else {
                out.println("No questions found.");
            }
            // Closing resources
            rs.close();
            stmt.close();
            con.close();
        } catch(Exception e) {
            out.println(e);
        }
    %>
</body>
</html>
