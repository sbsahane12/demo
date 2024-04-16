Slip 13: Java Programs - Database Information and Thread Lifecycle

1. Java program to display information about the database and list all the tables in the database using DatabaseMetaData:

This Java program uses DatabaseMetaData to display information about the database and list all the tables in the database.

```java
import java.sql.*;

public class DatabaseInfo {
    public static void main(String[] args) {
        // Database connection parameters
        String url = "jdbc:mysql://localhost:3306/your_database";
        String username = "username";
        String password = "password";

        try {
            // Establishing database connection
            Class.forName("com.mysql.jdbc.Driver");
            Connection con = DriverManager.getConnection(url, username, password);

            // Getting DatabaseMetaData
            DatabaseMetaData metaData = con.getMetaData();

            // Displaying database information
            System.out.println("Database Information:");
            System.out.println("Database Name: " + metaData.getDatabaseProductName());
            System.out.println("Database Version: " + metaData.getDatabaseProductVersion());
            System.out.println();

            // Listing all tables in the database
            ResultSet tables = metaData.getTables(null, null, "%", null);
            System.out.println("Tables in the Database:");
            while (tables.next()) {
                System.out.println(tables.getString("TABLE_NAME"));
            }

            // Closing resources
            tables.close();
            con.close();
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```
Q2:
```
public class ThreadLifecycleDemo {
    public static void main(String[] args) {
        ThreadLifecycle thread1 = new ThreadLifecycle("Thread 1");
        ThreadLifecycle thread2 = new ThreadLifecycle("Thread 2");

        thread1.start();
        thread2.start();
    }
}

class ThreadLifecycle extends Thread {
    public ThreadLifecycle(String name) {
        super(name);
    }

    public void run() {
        int sleepTime = (int) (Math.random() * 5000); // Random sleep time between 0 to 4999 milliseconds
        try {
            System.out.println("Thread " + getName() + " is created.");
            System.out.println("Thread " + getName() + " is going to sleep for " + sleepTime + " milliseconds.");
            sleep(sleepTime);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Thread " + getName() + " is dead.");
    }
}
```
