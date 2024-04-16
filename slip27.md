Slip 27: Java Program to Display College Details on JTable and Servlet Program to Change Session Inactive Time Interval

1. Java Program to display the details of College (CID, CName, address, Year) on JTable:

This Java program displays the details of College (CID, CName, address, Year) on a JTable. It fetches the data from the database and populates the JTable with the retrieved data.

```java
import javax.swing.*;
import java.sql.*;

public class CollegeDetails extends JFrame {
    private JTable table;

    public CollegeDetails() {
        setTitle("College Details");
        setSize(500, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        try {
            String url = "jdbc:mysql://localhost:3306/your_database";
            String username = "username";
            String password = "password";

            Connection con = DriverManager.getConnection(url, username, password);
            Statement stmt = con.createStatement();
            ResultSet rs = stmt.executeQuery("SELECT * FROM College");

            // Fetch number of rows
            rs.last();
            int rowCount = rs.getRow();
            rs.beforeFirst();

            // Create 2D array to hold table data
            Object[][] data = new Object[rowCount][4];

            // Populate data from ResultSet to 2D array
            int row = 0;
            while (rs.next()) {
                data[row][0] = rs.getInt("CID");
                data[row][1] = rs.getString("CName");
                data[row][2] = rs.getString("Address");
                data[row][3] = rs.getInt("Year");
                row++;
            }

            // Column names
            String[] columnNames = {"CID", "CName", "Address", "Year"};

            // Create JTable with data and column names
            table = new JTable(data, columnNames);
            JScrollPane scrollPane = new JScrollPane(table);
            getContentPane().add(scrollPane);

            con.close();
        } catch (Exception e) {
            e.printStackTrace();
        }

        setVisible(true);
    }

    public static void main(String[] args) {
        new CollegeDetails();
    }
}

```
Q2
```
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class SessionTimeoutServlet extends HttpServlet {
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Get the current session
        HttpSession session = request.getSession();

        // Set the inactive time interval to 60 seconds (1 minute)
        session.setMaxInactiveInterval(60);

        // Print message
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("<html><body>");
        out.println("<h3>Session inactive time interval changed to 1 minute.</h3>");
        out.println("</body></html>");
        out.close();
    }
}

```
