Slip 23: Java Program to Display Vowels and Student Names Using Command Line Input

1. Java program to accept a String from a user and display each vowel from a String after every 3 seconds using Thread.sleep:

This Java program accepts a String from a user and displays each vowel from the String after every 3 seconds using Thread.sleep.

```java
import java.util.Scanner;

public class DisplayVowels {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter a string: ");
        String input = scanner.nextLine();
        
        // Create a new thread to display vowels
        Thread vowelThread = new Thread(() -> {
            for (char ch : input.toCharArray()) {
                if ("AEIOUaeiou".indexOf(ch) != -1) {
                    System.out.println(ch);
                    try {
                        Thread.sleep(3000); // Sleep for 3 seconds
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        });

        // Start the thread
        vowelThread.start();

        // Ensure the main thread waits for the vowel thread to finish
        try {
            vowelThread.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        scanner.close();
    }
}
```
Q2
```
import java.util.*;

public class StudentNames {
    public static void main(String[] args) {
        List<String> studentNames = new ArrayList<>();
        
        // Accept 'N' student names through command line input
        for (String name : args) {
            studentNames.add(name);
        }

        // Display student names using Iterator
        System.out.println("Student Names (Using Iterator):");
        Iterator<String> iterator = studentNames.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }

        // Display student names in reverse order using ListIterator
        System.out.println("\nStudent Names (Using ListIterator in Reverse):");
        ListIterator<String> listIterator = studentNames.listIterator(studentNames.size());
        while (listIterator.hasPrevious()) {
            System.out.println(listIterator.previous());
        }
    }
}
```
