Slip 14: Java Program for Simple Search Engine and JSP Program to Calculate Sum of Digits

1. Java program for a simple search engine:

This Java program accepts a string to be searched and searches for the string in all text files in the current folder. It uses a separate thread for each file. The result displays the filename and line number where the string is found.

```java
import java.io.*;
import java.util.concurrent.*;

public class SimpleSearchEngine {
    public static void main(String[] args) {
        String searchString = "your_search_string"; // Enter the string to be searched here

        File folder = new File(".");
        File[] files = folder.listFiles();

        ExecutorService executor = Executors.newFixedThreadPool(files.length);

        for (File file : files) {
            if (file.isFile() && file.getName().endsWith(".txt")) {
                executor.execute(new SearchTask(file, searchString));
            }
        }

        executor.shutdown();
    }
}

class SearchTask implements Runnable {
    private File file;
    private String searchString;

    public SearchTask(File file, String searchString) {
        this.file = file;
        this.searchString = searchString;
    }

    public void run() {
        try {
            BufferedReader reader = new BufferedReader(new FileReader(file));
            String line;
            int lineNumber = 0;
            while ((line = reader.readLine()) != null) {
                lineNumber++;
                if (line.contains(searchString)) {
                    System.out.println("String found in file: " + file.getName() + ", line: " + lineNumber);
                }
            }
            reader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
Q2 : 
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Sum of Digits</title>
</head>
<body>
    <%
        int number = Integer.parseInt(request.getParameter("number"));
        int firstDigit = number;
        int lastDigit = number % 10;

        while (firstDigit >= 10) {
            firstDigit /= 10;
        }

        int sum = firstDigit + lastDigit;
    %>

    <h2>Sum of First and Last Digits</h2>
    <p style="color:red; font-size:18px;">Sum: <%= sum %></p>
</body>
</html>

```
