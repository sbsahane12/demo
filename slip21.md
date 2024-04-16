Slip 21: Java Program to Accept Subject Names and Display Using Iterator, and Java Program to Solve Producer-Consumer Problem

1. Java program to accept 'N' subject names from a user, store them into a LinkedList Collection, and display them using the Iterator interface:

This Java program accepts 'N' subject names from a user, stores them into a LinkedList Collection, and displays them using the Iterator interface.

```java
import java.util.*;

public class SubjectNames {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the number of subjects (N): ");
        int N = scanner.nextInt();

        LinkedList<String> subjects = new LinkedList<>();
        System.out.println("Enter " + N + " subject names:");
        for (int i = 0; i < N; i++) {
            String subject = scanner.next();
            subjects.add(subject);
        }

        System.out.println("Subject Names:");
        Iterator<String> iterator = subjects.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }

        scanner.close();
    }
}
```

Q2:
```
import java.util.*;

class Buffer {
    private int value;
    private boolean produced = false;

    // Method for producer to produce a value
    synchronized void produce(int val) {
        while (produced) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        value = val;
        System.out.println("Produced: " + value);
        produced = true;
        notify();
    }

    // Method for consumer to consume the value
    synchronized int consume() {
        while (!produced) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("Consumed: " + value);
        produced = false;
        notify();
        return value;
    }
}

class Producer extends Thread {
    private Buffer buffer;

    Producer(Buffer buffer) {
        this.buffer = buffer;
    }

    public void run() {
        for (int i = 1; i <= 5; i++) {
            buffer.produce(i);
            try {
                sleep(1000); // Sleep for 1 second
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class Consumer extends Thread {
    private Buffer buffer;

    Consumer(Buffer buffer) {
        this.buffer = buffer;
    }

    public void run() {
        for (int i = 1; i <= 5; i++) {
            buffer.consume();
        }
    }
}

public class ProducerConsumer {
    public static void main(String[] args) {
        Buffer buffer = new Buffer();
        Producer producer = new Producer(buffer);
        Consumer consumer = new Consumer(buffer);
        producer.start();
        consumer.start();
    }
}
