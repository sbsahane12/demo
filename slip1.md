Slip 1: Java Program to display alphabets and store employee details

1. Java program to display all the alphabets between 'A' to 'Z' after every 2 seconds:

```java
public class AlphabetDisplay {
    public static void main(String[] args) throws InterruptedException {
        char alphabet;
        for (alphabet = 'A'; alphabet <= 'Z'; alphabet++) {
            System.out.print(alphabet + " ");
            Thread.sleep(2000); // Sleep for 2 seconds
        }
    }
}
```

2)Java program to accept the details of Employee (Eno, EName, Designation, Salary) from a user and store it into the database (using Swing):
```
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class EmployeeDetailsForm extends JFrame implements ActionListener {
    private JTextField txtEno, txtEName, txtDesignation, txtSalary;

    public EmployeeDetailsForm() {
        setTitle("Employee Details Form");
        setSize(400, 300);
        setDefaultCloseOperation(EXIT_ON_CLOSE);

        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(5, 2));

        JLabel lblEno = new JLabel("Employee Number:");
        txtEno = new JTextField();
        JLabel lblEName = new JLabel("Employee Name:");
        txtEName = new JTextField();
        JLabel lblDesignation = new JLabel("Designation:");
        txtDesignation = new JTextField();
        JLabel lblSalary = new JLabel("Salary:");
        txtSalary = new JTextField();

        JButton btnSave = new JButton("Save");
        btnSave.addActionListener(this);

        panel.add(lblEno);
        panel.add(txtEno);
        panel.add(lblEName);
        panel.add(txtEName);
        panel.add(lblDesignation);
        panel.add(txtDesignation);
        panel.add(lblSalary);
        panel.add(txtSalary);
        panel.add(new JLabel()); // Empty label for layout
        panel.add(btnSave);

        add(panel);
        setVisible(true);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        String eno = txtEno.getText();
        String eName = txtEName.getText();
        String designation = txtDesignation.getText();
        String salary = txtSalary.getText();

        try {
            Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/your_database", "username", "password");
            String sql = "INSERT INTO employee (Eno, EName, Designation, Salary) VALUES (?, ?, ?, ?)";
            PreparedStatement statement = connection.prepareStatement(sql);
            statement.setString(1, eno);
            statement.setString(2, eName);
            statement.setString(3, designation);
            statement.setString(4, salary);
            statement.executeUpdate();
            JOptionPane.showMessageDialog(this, "Employee details saved successfully!");
            connection.close();
        } catch (SQLException ex) {
            ex.printStackTrace();
        }
    }

    public static void main(String[] args) {
        new EmployeeDetailsForm();
    }
}
```
