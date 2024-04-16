Slip 16: Java Program to Work with TreeSet and JDBC Program to Insert and Display Teacher Details

1. Java program to create a TreeSet, add some colors (String), and print out the content of TreeSet in ascending order:

This Java program creates a TreeSet, adds some colors (Strings), and prints out the content of the TreeSet in ascending order.

```java
import java.util.*;

public class TreeSetExample {
    public static void main(String[] args) {
        TreeSet<String> colors = new TreeSet<>();
        colors.add("Red");
        colors.add("Green");
        colors.add("Blue");
        colors.add("Yellow");
        colors.add("Orange");

        System.out.println("Colors in ascending order:");
        for (String color : colors) {
            System.out.println(color);
        }
    }
}
```
Q2:
```import java.sql.*;

public class TeacherDetails {
    public static void main(String[] args) {
        // Database connection parameters
        String url = "jdbc:mysql://localhost:3306/your_database";
        String username = "username";
        String password = "password";

        try {
            // Establishing database connection
            Class.forName("com.mysql.jdbc.Driver");
            Connection con = DriverManager.getConnection(url, username, password);
            
            // Inserting records into Teacher Table
            PreparedStatement pstmt = con.prepareStatement("INSERT INTO Teacher (TNo, TName, Subject) VALUES (?, ?, ?)");
            pstmt.setInt(1, 101);
            pstmt.setString(2, "John Doe");
            pstmt.setString(3, "JAVA");
            pstmt.executeUpdate();

            pstmt.setInt(1, 102);
            pstmt.setString(2, "Alice Smith");
            pstmt.setString(3, "C++");
            pstmt.executeUpdate();

            pstmt.setInt(1, 103);
            pstmt.setString(2, "Bob Johnson");
            pstmt.setString(3, "JAVA");
            pstmt.executeUpdate();

            pstmt.setInt(1, 104);
            pstmt.setString(2, "Emily Brown");
            pstmt.setString(3, "Python");
            pstmt.executeUpdate();

            pstmt.setInt(1, 105);
            pstmt.setString(2, "David Wilson");
            pstmt.setString(3, "JAVA");
            pstmt.executeUpdate();

            // Displaying details of teachers teaching "JAVA" subject
            PreparedStatement pstmtSelect = con.prepareStatement("SELECT * FROM Teacher WHERE Subject = ?");
            pstmtSelect.setString(1, "JAVA");
            ResultSet rs = pstmtSelect.executeQuery();
            System.out.println("Details of Teachers teaching JAVA:");
            while (rs.next()) {
                System.out.println("Teacher No: " + rs.getInt("TNo"));
                System.out.println("Teacher Name: " + rs.getString("TName"));
                System.out.println("Subject: " + rs.getString("Subject"));
                System.out.println();
            }

            // Closing resources
            rs.close();
            pstmtSelect.close();
            pstmt.close();
            con.close();
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```
