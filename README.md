Slip 1:
1. Java program to display all alphabets between 'A' to 'Z' after every 2 seconds:

```java
public class AlphabetDisplay {
    public static void main(String[] args) {
        char ch;
        for(ch = 'A'; ch <= 'Z'; ch++) {
            System.out.print(ch + " ");
            try {
                Thread.sleep(2000); // Sleep for 2 seconds
            } catch(InterruptedException e) {
                System.out.println(e);
            }
        }
    }
}

