Slip 11: HTML Page to Pass Customer Number to Servlet and Java Program to Display Column Information

1. Design an HTML page which passes customer number to a search servlet. The servlet searches for the customer number in a database (customer table) and returns customer details if found the number otherwise display an error message:

To implement this, you need to create an HTML page that contains a form to input the customer number. When the user submits the form, it should pass the customer number to a servlet. The servlet will then search for the customer number in the database and return the customer details if found, otherwise, it will display an error message.

Here's an example HTML page (search.html):

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Search Customer</title>
</head>
<body>
    <h2>Search Customer</h2>
    <form action="SearchServlet" method="GET">
        <label for="customerNumber">Enter Customer Number:</label>
        <input type="text" id="customerNumber" name="customerNumber" required>
        <button type="submit">Search</button>
    </form>
</body>
</html>

import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.sql.*;

public class SearchServlet extends HttpServlet {
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        
        // Retrieve the customer number from the request
        String customerNumber = request.getParameter("customerNumber");
        
        // Database connection parameters
        String url = "jdbc:postgresql://localhost:5432/your_database";
        String username = "username";
        String password = "password";
        
        try {
            // Establish database connection
            Class.forName("org.postgresql.Driver");
            Connection con = DriverManager.getConnection(url, username, password);
            
            // Prepare SQL statement to search for customer details
            String query = "SELECT * FROM customer WHERE customer_number = ?";
            PreparedStatement pstmt = con.prepareStatement(query);
            pstmt.setString(1, customerNumber);
            
            // Execute the query
            ResultSet rs = pstmt.executeQuery();
            
            if (rs.next()) {
                // Customer details found, display them
                out.println("<h2>Customer Details</h2>");
                out.println("<p>Customer Number: " + rs.getString("customer_number") + "</p>");
                out.println("<p>Name: " + rs.getString("name") + "</p>");
                out.println("<p>Email: " + rs.getString("email") + "</p>");
                out.println("<p>Phone: " + rs.getString("phone") + "</p>");
            } else {
                // Customer not found
                out.println("<h2>Error</h2>");
                out.println("<p>Customer with number " + customerNumber + " not found.</p>");
            }
            
            // Close resources
            rs.close();
            pstmt.close();
            con.close();
        } catch (Exception e) {
            out.println("<h2>Error</h2>");
            out.println("<p>An error occurred: " + e.getMessage() + "</p>");
        }
    }
}
```
Java program to display information about all columns in the DONAR table using ResultSetMetaData:
This program retrieves the metadata information about all columns in the DONAR table using ResultSetMetaData.
```
import java.sql.*;

public class DonarTableMetadata {
    public static void main(String[] args) {
        try {
            // Establishing database connection
            Class.forName("org.postgresql.Driver");
            Connection con = DriverManager.getConnection("jdbc:postgresql://localhost:5432/your_database", "username", "password");
            Statement stmt = con.createStatement();

            // Retrieving metadata about DONAR table columns
            ResultSet rs = stmt.executeQuery("SELECT * FROM DONAR");
            ResultSetMetaData rsmd = rs.getMetaData();

            // Displaying column information
            int columnCount = rsmd.getColumnCount();
            System.out.println("Column Information for DONAR table:");
            for (int i = 1; i <= columnCount; i++) {
                System.out.println("Column Name: " + rsmd.getColumnName(i));
                System.out.println("Data Type: " + rsmd.getColumnTypeName(i));
                System.out.println("Column Size: " + rsmd.getColumnDisplaySize(i));
                System.out.println("Is Nullable: " + (rsmd.isNullable(i) == ResultSetMetaData.columnNullable ? "Yes" : "No"));
                System.out.println();
            }

            // Closing resources
            rs.close();
            stmt.close();
            con.close();
        } catch (Exception e) {
            System.out.println(e);
        }
      }
}
```
