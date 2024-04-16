Slip 7: Java Programs - Multi-threaded Application and Database Operations

1. Java program that implements a multi-thread application with three threads:
    - The first thread generates a random integer number after every one second.
    - If the number is even, the second thread computes the square of that number and prints it.
    - If the number is odd, the third thread computes the cube of that number and prints it.

```java
import java.util.Random;

public class NumberProcessor {
    public static void main(String[] args) {
        RandomNumberGenerator generator = new RandomNumberGenerator();
        NumberSquared squareThread = new NumberSquared(generator);
        NumberCubed cubeThread = new NumberCubed(generator);

        generator.start();
        squareThread.start();
        cubeThread.start();
    }
}

class RandomNumberGenerator extends Thread {
    public void run() {
        Random random = new Random();
        while (true) {
            int randomNumber = random.nextInt(100); // Generating random number between 0 and 99
            System.out.println("Generated number: " + randomNumber);
            try {
                Thread.sleep(1000); // Sleep for 1 second
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class NumberSquared extends Thread {
    private RandomNumberGenerator generator;

    public NumberSquared(RandomNumberGenerator generator) {
        this.generator = generator;
    }

    public void run() {
        while (true) {
            int number = generator.getCurrentNumber();
            if (number % 2 == 0) {
                System.out.println("Square of " + number + ": " + (number * number));
            }
        }
    }
}

class NumberCubed extends Thread {
    private RandomNumberGenerator generator;

    public NumberCubed(RandomNumberGenerator generator) {
        this.generator = generator;
    }

    public void run() {
        while (true) {
            int number = generator.getCurrentNumber();
            if (number % 2 != 0) {
                System.out.println("Cube of " + number + ": " + (number * number * number));
            }
        }
    }
}
Java program for the following database operations:
i. To create a Product(Pid, Pname, Price) table.
ii. Insert at least five records into the table.
iii. Display all the records from the table.
java
Copy code
import java.sql.*;

public class ProductDatabaseOperations {
    public static void main(String[] args) {
        try {
            // Establishing database connection
            Class.forName("org.postgresql.Driver");
            Connection con = DriverManager.getConnection("jdbc:postgresql://localhost:5432/your_database", "username", "password");
            Statement stmt = con.createStatement();

            // i. Creating Product table
            String createTableQuery = "CREATE TABLE IF NOT EXISTS Product (Pid SERIAL PRIMARY KEY, Pname VARCHAR(255), Price NUMERIC)";
            stmt.executeUpdate(createTableQuery);
            System.out.println("Product table created successfully.");

            // ii. Inserting records into the table
            String[] products = {"Apple", "Banana", "Orange", "Mango", "Grapes"};
            double[] prices = {1.99, 0.99, 2.49, 1.79, 3.99};
            for (int i = 0; i < products.length; i++) {
                String insertQuery = "INSERT INTO Product (Pname, Price) VALUES ('" + products[i] + "', " + prices[i] + ")";
                stmt.executeUpdate(insertQuery);
            }
            System.out.println("Records inserted into Product table successfully.");

            // iii. Displaying all records from the table
            ResultSet rs = stmt.executeQuery("SELECT * FROM Product");
            System.out.println("ProductID\tProductName\tPrice");
            while (rs.next()) {
                System.out.println(rs.getInt("Pid") + "\t\t" + rs.getString("Pname") + "\t\t" + rs.getDouble("Price"));
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
