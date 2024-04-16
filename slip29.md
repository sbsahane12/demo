Slip 29: Java Program to Display Column Information and Manipulate LinkedList

1. Java program to display information about all columns in the DONAR table using ResultSetMetaData:

This Java program retrieves metadata about all columns in the DONAR table using ResultSetMetaData and displays it.

```java
import java.sql.*;

public class ColumnInformation {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/your_database";
        String username = "username";
        String password = "password";

        try {
            Connection con = DriverManager.getConnection(url, username, password);
            Statement stmt = con.createStatement();
            ResultSet rs = stmt.executeQuery("SELECT * FROM DONAR");

            ResultSetMetaData rsmd = rs.getMetaData();
            int columnCount = rsmd.getColumnCount();

            System.out.println("Column Information:");
            for (int i = 1; i <= columnCount; i++) {
                System.out.println("Column Name: " + rsmd.getColumnName(i));
                System.out.println("Column Type: " + rsmd.getColumnTypeName(i));
                System.out.println("Column Size: " + rsmd.getColumnDisplaySize(i));
                System.out.println("-----------------------------");
            }

            con.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
Q2:
```
import java.util.LinkedList;

public class LinkedListOperations {
    public static void main(String[] args) {
        // Create LinkedList of Integer objects
        LinkedList<Integer> linkedList = new LinkedList<>();

        // Add element at first position
        linkedList.addFirst(10);
        linkedList.addFirst(20);
        linkedList.addFirst(30);

        // Delete last element
        linkedList.removeLast();

        // Display the size of the linked list
        System.out.println("Size of LinkedList: " + linkedList.size());
    }
}
```
