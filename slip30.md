Slip 30: Java Program for Synchronization and Scrollable ResultSet Implementation

1. Java program for the implementation of synchronization:

This Java program demonstrates synchronization using the synchronized keyword to ensure thread safety when multiple threads are accessing a shared resource.

```java
public class SynchronizationDemo {
    private int counter = 0;

    // Synchronized method to increment counter
    public synchronized void increment() {
        counter++;
    }

    public static void main(String[] args) throws InterruptedException {
        SynchronizationDemo demo = new SynchronizationDemo();

        // Create multiple threads to increment counter
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                demo.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                demo.increment();
            }
        });

        // Start the threads
        t1.start();
        t2.start();

        // Join the threads to ensure they finish execution before printing the counter value
        t1.join();
        t2.join();

        // Print the counter value
        System.out.println("Counter value: " + demo.counter);
    }
}
```
Q2:
```
import java.sql.*;

public class ScrollableResultSetDemo {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/your_database";
        String username = "username";
        String password = "password";

        try {
            Connection con = DriverManager.getConnection(url, username, password);
            Statement stmt = con.createStatement(ResultSet.TYPE_SCROLL_SENSITIVE, ResultSet.CONCUR_READ_ONLY);
            ResultSet rs = stmt.executeQuery("SELECT * FROM Teacher");

            // Move cursor to the last row
            rs.last();

            // Print details of the last row
            System.out.println("Last Teacher Details:");
            System.out.println("TID: " + rs.getInt("TID"));
            System.out.println("TName: " + rs.getString("TName"));
            System.out.println("Salary: " + rs.getDouble("Salary"));

            // Move cursor to the first row
            rs.first();

            // Print details of the first row
            System.out.println("\nFirst Teacher Details:");
            System.out.println("TID: " + rs.getInt("TID"));
            System.out.println("TName: " + rs.getString("TName"));
            System.out.println("Salary: " + rs.getDouble("Salary"));

            con.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
