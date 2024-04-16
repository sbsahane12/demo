Slip 12: JSP Program to Check Perfect Number and Java Program to Work with Database Table

1. JSP program to check whether a given number is Perfect or not using Include directive:

This JSP program checks whether a given number is a Perfect number or not. It includes another JSP file that contains the logic to determine if a number is Perfect or not.

Create a main JSP file (checkPerfect.jsp) that includes the logic to check for a Perfect number and displays the result.

```jsp

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Perfect Number Checker</title>
</head>
<body>
    <h2>Perfect Number Checker</h2>
    <%
        int number = Integer.parseInt(request.getParameter("number"));
        request.setAttribute("number", number);
        request.getRequestDispatcher("perfectCheck.jsp").include(request, response);
    %>
</body>
</html>

```
perfectCheck.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.util.*" %>
<%@ page import="java.io.*" %>
<%@ page import="java.lang.*" %>
<%
    int number = (Integer)request.getAttribute("number");
    int sum = 0;
    for (int i = 1; i < number; i++) {
        if (number % i == 0) {
            sum += i;
        }
    }
    if (sum == number) {
        out.println(number + " is a Perfect number.");
    } else {
        out.println(number + " is not a Perfect number.");
    }
%>
````


2 Questionm Answer
```
import java.sql.*;
import javax.swing.*;
import java.awt.*;

public class ProjectTableDisplay extends JFrame {
    private JScrollPane scrollPane;
    private JTable table;

    public ProjectTableDisplay() {
        setTitle("Project Table Details");
        setSize(600, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        // Creating database connection parameters
        String url = "jdbc:mysql://localhost:3306/your_database";
        String username = "username";
        String password = "password";

        try {
            // Establishing database connection
            Class.forName("com.mysql.jdbc.Driver");
            Connection con = DriverManager.getConnection(url, username, password);
            Statement stmt = con.createStatement();

            // Creating PROJECT table
            String createTableQuery = "CREATE TABLE IF NOT EXISTS PROJECT (project_id INT PRIMARY KEY, Project_name VARCHAR(255), Project_description VARCHAR(255), Project_Status VARCHAR(50))";
            stmt.executeUpdate(createTableQuery);
            System.out.println("PROJECT table created successfully.");

            // Inserting values into PROJECT table
            String[] projectIds = {"1", "2", "3"};
            String[] projectNames = {"Project A", "Project B", "Project C"};
            String[] projectDescriptions = {"Description A", "Description B", "Description C"};
            String[] projectStatuses = {"Active", "Inactive", "Completed"};
            for (int i = 0; i < projectIds.length; i++) {
                String insertQuery = "INSERT INTO PROJECT (project_id, Project_name, Project_description, Project_Status) VALUES ('" + projectIds[i] + "', '" + projectNames[i] + "', '" + projectDescriptions[i] + "', '" + projectStatuses[i] + "')";
                stmt.executeUpdate(insertQuery);
            }
            System.out.println("Records inserted into PROJECT table successfully.");

            // Retrieving all records from PROJECT table
            ResultSet rs = stmt.executeQuery("SELECT * FROM PROJECT");

            // Creating JTable to display records
            ResultSetMetaData rsmd = rs.getMetaData();
            int columnCount = rsmd.getColumnCount();
            String[] columnNames = new String[columnCount];
            for (int i = 0; i < columnCount; i++) {
                columnNames[i] = rsmd.getColumnName(i + 1);
            }

            String[][] data = new String[3][columnCount]; // Assuming 3 records for now
            int rowIndex = 0;
            while (rs.next()) {
                for (int i = 0; i < columnCount; i++) {
                    data[rowIndex][i] = rs.getString(i + 1);
                }
                rowIndex++;
            }

            table = new JTable(data, columnNames);
            scrollPane = new JScrollPane(table);
            add(scrollPane, BorderLayout.CENTER);

            // Closing resources
            rs.close();
            stmt.close();
            con.close();
        } catch (Exception e) {
            System.out.println(e);
        }

        setVisible(true);
    }

    public static void main(String[] args) {
        new ProjectTableDisplay();
    }
}
```

