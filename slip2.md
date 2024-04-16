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
2)Servlet to provide information about an HTTP request and server details:
```
Copy code
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Enumeration;

@WebServlet("/requestinfo")
public class RequestInfoServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();

        // Client request information
        out.println("<h2>Client Request Information:</h2>");
        out.println("<strong>IP Address:</strong> " + request.getRemoteAddr() + "<br>");
        out.println("<strong>Browser Type:</strong> " + request.getHeader("User-Agent") + "<br>");

        // Server information
        out.println("<h2>Server Information:</h2>");
        out.println("<strong>Operating System Type:</strong> " + System.getProperty("os.name") + "<br>");

        // Loaded servlets
        out.println("<h2>Loaded Servlets:</h2>");
        Enumeration<String> servletNames = getServletContext().getServletRegistrationNames();
        while (servletNames.hasMoreElements()) {
            out.println("<strong>Servlet Name:</strong> " + servletNames.nextElement() + "<br>");
        }
    }
}
```
