Slip 17: Java Program to Accept and Display Sorted Integers and Multithreading Program to Display Numbers in a TextField

1. Java program to accept 'N' integers from a user, store and display integers in sorted order using an appropriate collection class. The collection should not accept duplicate elements:

This Java program accepts 'N' integers from a user, stores them in a collection (TreeSet) to automatically maintain them in sorted order and avoid duplicate elements, and then displays the sorted integers.

```java
import java.util.*;

public class SortedIntegerCollection {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the number of integers (N): ");
        int N = scanner.nextInt();

        TreeSet<Integer> numbers = new TreeSet<>();
        System.out.println("Enter " + N + " integers:");
        for (int i = 0; i < N; i++) {
            int num = scanner.nextInt();
            numbers.add(num);
        }

        System.out.println("Sorted integers:");
        for (int num : numbers) {
            System.out.println(num);
        }

        scanner.close();
    }
}
```
Q2:
```import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class NumberDisplay extends JFrame implements ActionListener, Runnable {
    private JTextField textField;
    private JButton startButton, stopButton;
    private Thread thread;

    public NumberDisplay() {
        setTitle("Number Display");
        setSize(300, 100);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        textField = new JTextField(20);
        startButton = new JButton("Start");
        stopButton = new JButton("Stop");

        startButton.addActionListener(this);
        stopButton.addActionListener(this);

        JPanel panel = new JPanel();
        panel.add(textField);
        panel.add(startButton);
        panel.add(stopButton);
        add(panel);

        setVisible(true);
    }

    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == startButton) {
            if (thread == null) {
                thread = new Thread(this);
                thread.start();
            }
        } else if (e.getSource() == stopButton) {
            if (thread != null) {
                thread.interrupt();
                thread = null;
            }
        }
    }

    public void run() {
        try {
            for (int i = 1; i <= 100; i++) {
                textField.setText(String.valueOf(i));
                Thread.sleep(1000); // Sleep for 1 second
            }
        } catch (InterruptedException e) {
            // Thread interrupted
        }
    }

    public static void main(String[] args) {
        new NumberDisplay();
    }
}
```
