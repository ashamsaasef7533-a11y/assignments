# GUI input and ActionListeners

### Armaan Shamsaasef
### Date: 11/9/2025
### CISC191 Week 10 Assignments


# Here is the flowchart!

<img width="569" height="930" alt="image" src="https://github.com/user-attachments/assets/eb987e39-b9ea-4d76-8634-95f6d182174e" />


## Explanation of my code.

This Java program creates a simple GUI salary calculator that takes user input for hourly wage and weekly hours through text fields. When the user clicks the Calculate button, the program reads both input values, validates they are positive numbers, then computes the yearly salary by multiplying hourly wage by hours per week by 52 weeks. The result is displayed formatted as currency in the interface, with error handling for invalid inputs like non-numbers or negative values. The entire application runs in a single window with a clean, user-friendly layout.

## Challenges

GUI Layout Management: Deciding between different layout managers (FlowLayout vs GridLayout) to create an intuitive interface

Event Handling: Ensuring the ActionListener properly captures button clicks and retrieves input data

Input Validation: Handling potential NumberFormatException when users enter non-numeric values

Component Positioning: Arranging labels, text fields, and button in a visually appealing way

Error Handling: Implementing try-catch blocks to gracefully handle invalid input

## This is my code!

```.java

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class SalaryCalculatorGUI extends JFrame {
    private JTextField hourlyWageField;
    private JTextField hoursPerWeekField;
    private JLabel resultLabel;

    public SalaryCalculatorGUI() {
        // Set up the main window
        setTitle("Salary Calculator");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(400, 300);
        setLocationRelativeTo(null);

        // Create main panel
        JPanel mainPanel = new JPanel();
        mainPanel.setLayout(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        mainPanel.setBorder(BorderFactory.createEmptyBorder(20, 20, 20, 20));

        // Title
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridwidth = 2;
        gbc.insets = new Insets(0, 0, 20, 0);
        JLabel titleLabel = new JLabel("Calculate Yearly Salary");
        titleLabel.setFont(new Font("Arial", Font.BOLD, 18));
        titleLabel.setHorizontalAlignment(SwingConstants.CENTER);
        mainPanel.add(titleLabel, gbc);

        // Hourly Wage
        gbc.gridx = 0;
        gbc.gridy = 1;
        gbc.gridwidth = 1;
        gbc.insets = new Insets(5, 5, 5, 5);
        gbc.anchor = GridBagConstraints.EAST;
        mainPanel.add(new JLabel("Hourly Wage:"), gbc);

        gbc.gridx = 1;
        gbc.gridy = 1;
        gbc.anchor = GridBagConstraints.WEST;
        hourlyWageField = new JTextField(15);
        mainPanel.add(hourlyWageField, gbc);

        // Hours per Week
        gbc.gridx = 0;
        gbc.gridy = 2;
        gbc.anchor = GridBagConstraints.EAST;
        mainPanel.add(new JLabel("Hours per Week:"), gbc);

        gbc.gridx = 1;
        gbc.gridy = 2;
        gbc.anchor = GridBagConstraints.WEST;
        hoursPerWeekField = new JTextField(15);
        mainPanel.add(hoursPerWeekField, gbc);

        // Calculate Button
        gbc.gridx = 0;
        gbc.gridy = 3;
        gbc.gridwidth = 2;
        gbc.insets = new Insets(20, 5, 20, 5);
        gbc.anchor = GridBagConstraints.CENTER;
        JButton calculateButton = new JButton("Calculate Yearly Salary");
        calculateButton.setFont(new Font("Arial", Font.BOLD, 14));
        calculateButton.setPreferredSize(new Dimension(200, 35));
        mainPanel.add(calculateButton, gbc);

        // Result Label
        gbc.gridx = 0;
        gbc.gridy = 4;
        gbc.gridwidth = 2;
        gbc.insets = new Insets(10, 5, 5, 5);
        resultLabel = new JLabel("Yearly Salary will appear here");
        resultLabel.setFont(new Font("Arial", Font.BOLD, 16));
        resultLabel.setForeground(Color.BLUE);
        resultLabel.setHorizontalAlignment(SwingConstants.CENTER);
        mainPanel.add(resultLabel, gbc);

        // Add action listener with debug output
        calculateButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println("Calculate button clicked!"); // Debug
                calculateSalary();
            }
        });

        // Also add Enter key listeners to text fields
        ActionListener enterListener = new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println("Enter key pressed!"); // Debug
                calculateSalary();
            }
        };

        hourlyWageField.addActionListener(enterListener);
        hoursPerWeekField.addActionListener(enterListener);

        add(mainPanel);
    }

    private void calculateSalary() {
        System.out.println("calculateSalary method called!"); // Debug

        try {
            // Get input values
            String wageText = hourlyWageField.getText().trim();
            String hoursText = hoursPerWeekField.getText().trim();

            System.out.println("Wage text: '" + wageText + "'"); // Debug
            System.out.println("Hours text: '" + hoursText + "'"); // Debug

            // Validate that fields are not empty
            if (wageText.isEmpty() || hoursText.isEmpty()) {
                resultLabel.setText("Please enter values in both fields");
                resultLabel.setForeground(Color.RED);
                JOptionPane.showMessageDialog(this,
                        "Please enter values for both fields.",
                        "Input Required",
                        JOptionPane.WARNING_MESSAGE);
                return;
            }

            // Parse input values
            double hourlyWage = Double.parseDouble(wageText);
            double hoursPerWeek = Double.parseDouble(hoursText);

            System.out.println("Parsed values: " + hourlyWage + ", " + hoursPerWeek); // Debug

            // Validate positive values
            if (hourlyWage <= 0 || hoursPerWeek <= 0) {
                resultLabel.setText("Please enter positive numbers");
                resultLabel.setForeground(Color.RED);
                JOptionPane.showMessageDialog(this,
                        "Please enter positive numbers greater than zero.",
                        "Invalid Input",
                        JOptionPane.ERROR_MESSAGE);
                return;
            }

            // Validate reasonable hours
            if (hoursPerWeek > 168) {
                resultLabel.setText("Hours cannot exceed 168 per week");
                resultLabel.setForeground(Color.RED);
                JOptionPane.showMessageDialog(this,
                        "Hours per week cannot exceed 168.",
                        "Invalid Input",
                        JOptionPane.ERROR_MESSAGE);
                return;
            }

            // Calculate yearly salary
            double yearlySalary = hourlyWage * hoursPerWeek * 52;

            // Display result
            String formattedSalary = String.format("$%,.2f", yearlySalary);
            resultLabel.setText("Yearly Salary: " + formattedSalary);
            resultLabel.setForeground(Color.BLUE);

            System.out.println("Calculation successful: " + formattedSalary); // Debug

        } catch (NumberFormatException e) {
            resultLabel.setText("Please enter valid numbers");
            resultLabel.setForeground(Color.RED);
            JOptionPane.showMessageDialog(this,
                    "Please enter valid numbers in both fields.",
                    "Input Error",
                    JOptionPane.ERROR_MESSAGE);
            System.out.println("NumberFormatException caught"); // Debug
        }
    }

    public static void main(String[] args) {
        // Use try-catch to handle any initialization errors
        try {
            System.out.println("Starting application..."); // Debug

            SwingUtilities.invokeLater(new Runnable() {
                @Override
                public void run() {
                    SalaryCalculatorGUI calculator = new SalaryCalculatorGUI();
                    calculator.setVisible(true);
                    System.out.println("GUI is now visible"); // Debug
                }
            });

        } catch (Exception e) {
            System.err.println("Error starting application: " + e.getMessage());
            e.printStackTrace();
        }
    }
}

```

## This is my result!

<img width="377" height="284" alt="image" src="https://github.com/user-attachments/assets/8cefa218-39c0-45b3-ad24-65a32b87eb1a" />


<img width="375" height="287" alt="image" src="https://github.com/user-attachments/assets/9b046e2c-f08d-4e2b-99b0-5d8f5fd3757d" />


```.out

Starting application...
GUI is now visible
Calculate button clicked!
calculateSalary method called!
Wage text: '100'
Hours text: '40'
Parsed values: 100.0, 40.0
Calculation successful: $208,000.00

Process finished with exit code 0


```


# VIDEO EXPLANATION!

https://app.screencastify.com/watch/paWut52NG7T1sqJZKDQk?checkOrg=30498c8e-8a1a-49cb-811f-fc81a0db6696






