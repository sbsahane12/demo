Slip 3: JSP Program and Java LinkedList Operations

1. JSP program to display the details of Patient (PNo, PName, Address, age, disease) in tabular form on the browser:

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Patient Details</title>
</head>
<body>
    <h2>Patient Details</h2>
    <table border="1">
        <tr>
            <th>Patient Number</th>
            <th>Name</th>
            <th>Address</th>
            <th>Age</th>
            <th>Disease</th>
        </tr>
        <%
            // Sample data for demonstration
            String[][] patients = {{"1", "John Doe", "123 Main St", "35", "Fever"},
                                    {"2", "Jane Smith", "456 Elm St", "28", "Cough"}};
            
            // Displaying patient details in tabular form
            for (String[] patient : patients) {
        %>
        <tr>
            <td><%= patient[0] %></td>
            <td><%= patient[1] %></td>
            <td><%= patient[2] %></td>
            <td><%= patient[3] %></td>
            <td><%= patient[4] %></td>
        </tr>
        <% } %>
    </table>
</body>
</html>
```
1)Using Database
Slip 3: JSP Program and Java LinkedList Operations

1. JSP program to display the details of Patient (PNo, PName, Address, age, disease) in tabular form on the browser:

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Patient Details</title>
</head>
<body>
    <h2>Patient Details</h2>
    <table border="1">
        <tr>
            <th>Patient Number</th>
            <th>Name</th>
            <th>Address</th>
            <th>Age</th>
            <th>Disease</th>
        </tr>
        <%
            try {
                // Establishing database connection
                Class.forName("com.mysql.jdbc.Driver");
                Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/your_database", "username", "password");
                Statement stmt = con.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT * FROM patients");
                
                // Displaying patient details from database in tabular form
                while (rs.next()) {
        %>
        <tr>
            <td><%= rs.getString("PNo") %></td>
            <td><%= rs.getString("PName") %></td>
            <td><%= rs.getString("Address") %></td>
            <td><%= rs.getString("Age") %></td>
            <td><%= rs.getString("Disease") %></td>
        </tr>
        <%
                }
                // Closing resources
                rs.close();
                stmt.close();
                con.close();
            } catch(Exception e) {
                out.println(e);
            }
        %>
    </table>
</body>
</html>
```
2)Java program to create LinkedList of String objects and perform the following operations:

```
import java.util.LinkedList;

public class LinkedListOperations {
    public static void main(String[] args) {
        // Creating a LinkedList of String objects
        LinkedList<String> linkedList = new LinkedList<>();

        // i. Add element at the end of the list
        linkedList.add("Apple");
        linkedList.add("Banana");
        linkedList.add("Orange");

        // Displaying initial contents of the list
        System.out.println("Initial LinkedList: " + linkedList);

        // ii. Delete first element of the list
        if (!linkedList.isEmpty()) {
            linkedList.removeFirst();
            System.out.println("After removing first element: " + linkedList);
        } else {
            System.out.println("LinkedList is empty.");
        }

        // iii. Display the contents of list in reverse order
        System.out.println("Contents of list in reverse order:");
        for (int i = linkedList.size() - 1; i >= 0; i--) {
            System.out.println(linkedList.get(i));
        }
    }
}
```
