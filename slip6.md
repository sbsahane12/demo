Slip 6: Java Programs - Collection for Integers and Simulating Traffic Signal with Threads

1. Java program to accept 'n' integers from the user and store them in a collection. Display them in sorted order. The collection should not accept duplicate elements:

```java
import java.util.*;

public class SortedIntegers {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Set<Integer> numbersSet = new TreeSet<>(); // Using TreeSet to automatically sort and remove duplicates

        System.out.print("Enter the number of integers (n): ");
        int n = scanner.nextInt();

        System.out.println("Enter " + n + " integers:");
        for (int i = 0; i < n; i++) {
            int num = scanner.nextInt();
            numbersSet.add(num);
        }

        System.out.println("Integers in sorted order (without duplicates):");
        for (int num : numbersSet) {
            System.out.println(num);
        }
    }
}
Java program to simulate traffic signal using threads:
java
Copy code
public class TrafficSignalSimulator {
    public static void main(String[] args) {
        TrafficSignal trafficSignal = new TrafficSignal();

        Thread car1 = new Thread(new Car(trafficSignal, "Car 1", 3)); // Car 1 arrives every 3 seconds
        Thread car2 = new Thread(new Car(trafficSignal, "Car 2", 5)); // Car 2 arrives every 5 seconds
        Thread car3 = new Thread(new Car(trafficSignal, "Car 3", 7)); // Car 3 arrives every 7 seconds

        car1.start();
        car2.start();
        car3.start();
    }
}

class TrafficSignal {
    private volatile boolean greenLight = true;

    public synchronized void waitAtSignal(String carName) {
        while (!greenLight) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println(carName + " is passing through the green light.");
    }

    public synchronized void changeSignal() {
        greenLight = !greenLight;
        if (greenLight) {
            System.out.println("Green Light ON");
        } else {
            System.out.println("Red Light ON");
        }
        notifyAll();
    }
}

class Car implements Runnable {
    private TrafficSignal trafficSignal;
    private String carName;
    private int arrivalTimeInSeconds;

    public Car(TrafficSignal trafficSignal, String carName, int arrivalTimeInSeconds) {
        this.trafficSignal = trafficSignal;
        this.carName = carName;
        this.arrivalTimeInSeconds = arrivalTimeInSeconds;
    }

    @Override
    public void run() {
        while (true) {
            trafficSignal.waitAtSignal(carName);
            try {
                Thread.sleep(1000 * arrivalTimeInSeconds); // Converting arrival time to milliseconds
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            trafficSignal.changeSignal();
        }
    }
}
