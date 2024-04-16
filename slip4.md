Slip 4: Java Programs - Runnable Interface and City Names with STD Codes

1. Java program using Runnable interface to blink Text on the frame:

```java
import javax.swing.*;

public class BlinkingText extends JFrame implements Runnable {
    private JLabel label;

    public BlinkingText() {
        setTitle("Blinking Text");
        setSize(300, 100);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(null);

        label = new JLabel("Blinking Text");
        label.setBounds(10, 10, 200, 30);
        add(label);

        Thread thread = new Thread(this);
        thread.start();

        setVisible(true);
    }

    @Override
    public void run() {
        try {
            while (true) {
                label.setVisible(false);
                Thread.sleep(500); // 500 milliseconds
                label.setVisible(true);
                Thread.sleep(500); // 500 milliseconds
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        new BlinkingText();
    }
}
Java program to store city names and their STD codes using an appropriate collection and perform the following operations:
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class CitySTD {
    public static void main(String[] args) {
        Map<String, String> cityCodes = new HashMap<>();
        Scanner scanner = new Scanner(System.in);
        
        // i. Add a new city and its code (No duplicates)
        cityCodes.put("New York", "212");
        cityCodes.put("Los Angeles", "213");
        cityCodes.put("Chicago", "312");

        // ii. Remove a city from the collection
        System.out.print("Enter the city to remove: ");
        String cityToRemove = scanner.nextLine();
        if (cityCodes.containsKey(cityToRemove)) {
            cityCodes.remove(cityToRemove);
            System.out.println(cityToRemove + " and its code removed successfully.");
        } else {
            System.out.println("City not found.");
        }

        // iii. Search for a city name and display the code
        System.out.print("Enter the city to search: ");
        String cityToSearch = scanner.nextLine();
        if (cityCodes.containsKey(cityToSearch)) {
            System.out.println("STD code for " + cityToSearch + ": " + cityCodes.get(cityToSearch));
        } else {
            System.out.println("City not found.");
        }
    }
}
