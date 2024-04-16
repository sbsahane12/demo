Slip 8: Java Program for Thread Printing and JSP Program for Prime Number Check

1. Java program to define a thread for printing text on the output screen for 'n' number of times. Create 3 threads and run them. Pass the text 'n' parameters to the thread constructor.

```java
public class TextPrinterThread extends Thread {
    private String text;
    private int times;

    public TextPrinterThread(String text, int times) {
        this.text = text;
        this.times = times;
    }

    public void run() {
        for (int i = 0; i < times; i++) {
            System.out.println(text);
        }
    }

    public static void main(String[] args) {
        TextPrinterThread thread1 = new TextPrinterThread("COVID19", 10);
        TextPrinterThread thread2 = new TextPrinterThread("LOCKDOWN2020", 20);
        TextPrinterThread thread3 = new TextPrinterThread("VACCINATED2021", 30);

        thread1.start();
        thread2.start();
        thread3.start();
    }
}
JSP program to check whether a given number is prime or not. Display the result in red color.
jsp
Copy code
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.io.*, java.util.*" %>
<%@ page import="java.lang.*" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Prime Number Check</title>
</head>
<body>
    <h2>Prime Number Check</h2>
    <%!
        boolean isPrime(int n) {
            if (n <= 1) {
                return false;
            }
            for (int i = 2; i <= Math.sqrt(n); i++) {
                if (n % i == 0) {
                    return false;
                }
            }
            return true;
        }
    %>
    <% 
        int num = Integer.parseInt(request.getParameter("number"));
        boolean prime = isPrime(num);
        String result = prime ? "Prime" : "Not Prime";
        String color = prime ? "red" : "black";
    %>
    <p style="color:<%= color %>;">The number <%= num %> is <%= result %>.</p>
</body>
</html>
