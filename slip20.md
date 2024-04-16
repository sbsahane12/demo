Slip 20: JSP Page to Display Number in Words and Java Program to Blink Image on JFrame Continuously

1. JSP page to accept a number from a user and display it in words, with the output in red color:

This JSP page accepts a number from a user and displays it in words. For example, if the user enters "123", it will be displayed as "One Two Three". The output is displayed in red color.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Number in Words</title>
</head>
<body>
    <h2 style="color:red;">Enter a Number:</h2>
    <form action="NumberInWords.jsp" method="post">
        <input type="text" name="number" required>
        <input type="submit" value="Submit">
    </form>
    
    <%
        String numberStr = request.getParameter("number");
        if (numberStr != null && !numberStr.isEmpty()) {
            String[] words = {"Zero", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine"};
            out.println("<h2 style='color:red;'>Number in Words:</h2>");
            for (char digit : numberStr.toCharArray()) {
                int index = Character.getNumericValue(digit);
                out.print(words[index] + " ");
            }
        }
    %>
</body>
</html>
```
Q2:
```import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class BlinkingImage extends JFrame implements ActionListener {
    private Timer timer;
    private JLabel imageLabel;
    private ImageIcon imageIcon;
    private boolean isBlinking = false;

    public BlinkingImage() {
        setTitle("Blinking Image");
        setSize(300, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        imageIcon = new ImageIcon("image.png");
        imageLabel = new JLabel(imageIcon);
        add(imageLabel);

        timer = new Timer(500, this);
        timer.start();

        setVisible(true);
    }

    public void actionPerformed(ActionEvent e) {
        if (isBlinking) {
            imageLabel.setVisible(false);
        } else {
            imageLabel.setVisible(true);
        }
        isBlinking = !isBlinking;
    }

    public static void main(String[] args) {
        new BlinkingImage();
    }
}
```
