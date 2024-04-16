Slip 2: Java Program to read names and servlet design

1. Java program to read 'N' names of your friends, store it into HashSet, and display them in ascending order:

```java
import java.util.HashSet;
import java.util.Scanner;
import java.util.Set;
import java.util.TreeSet;

public class FriendsNames {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter the number of friends (N): ");
        int n = scanner.nextInt();

        Set<String> friendsSet = new HashSet<>();

        System.out.println("Enter the names of your friends:");
        for (int i = 0; i < n; i++) {
            System.out.print("Friend " + (i + 1) + ": ");
            String name = scanner.next();
            friendsSet.add(name);
        }

        TreeSet<String> sortedFriends = new TreeSet<>(friendsSet);

        System.out.println("\nList of friends in ascending order:");
        for (String friend : sortedFriends) {
            System.out.println(friend);
        }
    }
}
```
