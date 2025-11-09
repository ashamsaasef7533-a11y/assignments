# GUI input and ActionListeners

### Armaan Shamsaasef
### Date: 11/9/2025
### CISC191 Week 10 Assignments


# Here is the flowchart!

<img width="839" height="899" alt="image" src="https://github.com/user-attachments/assets/d7e38479-15b0-4767-b3e0-6a69f6008feb" />


## Explanation of my code.

This Java program creates a distance converter GUI that takes a single input of miles through a formatted text field. When the user clicks the Convert button, the program reads the mile value and performs three simultaneous conversions: miles to kilometers using the factor 1.60934, kilometers to meters by multiplying by 1000, and miles to feet using the factor 5280. The results are instantly displayed in three separate labeled fields, allowing users to see all converted distances at once. The formatted text field ensures only valid numeric input is accepted, making the interface user-friendly and error-resistant.

## Challenges

JFormattedTextField Setup: Configuring the NumberFormatter with proper bounds and validation rules required careful research

Conversion Accuracy: Ensuring the distance conversion factors were precise and correctly applied

Layout Management: Arranging multiple result labels clearly while maintaining a clean interface

Input Validation: Using formatted text field to restrict input to valid numbers while providing user feedback

Number Formatting: Properly displaying results with consistent decimal formatting for all units

## This is my code!

```.java

import javax.swing.*;
import javax.swing.text.NumberFormatter;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.text.NumberFormat;

public class DistanceConverter extends JFrame {
    private JFormattedTextField milesField;
    private JLabel kmResult, metersResult, feetResult;

    public DistanceConverter() {
        // Set up the main window
        setTitle("Distance Converter");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(700, 700);
        setLocationRelativeTo(null);

        // Create main panel
        JPanel mainPanel = new JPanel();
        mainPanel.setLayout(new GridLayout(6, 1, 10, 10));
        mainPanel.setBorder(BorderFactory.createEmptyBorder(20, 20, 20, 20));

        // Title
        JLabel titleLabel = new JLabel("Distance Converter", JLabel.CENTER);
        titleLabel.setFont(new Font("Arial", Font.BOLD, 24));
        mainPanel.add(titleLabel);

        // Input label
        JLabel inputLabel = new JLabel("Enter Distance in Miles:", JLabel.CENTER);
        mainPanel.add(inputLabel);

        // Formatted text field for miles input
        NumberFormat format = NumberFormat.getNumberInstance();
        NumberFormatter formatter = new NumberFormatter(format);
        formatter.setValueClass(Double.class);
        formatter.setMinimum(0.0);
        formatter.setMaximum(10000.0);
        formatter.setAllowsInvalid(false);

        milesField = new JFormattedTextField(formatter);
        milesField.setValue(0.0);
        milesField.setHorizontalAlignment(JTextField.CENTER);
        milesField.setFont(new Font("Arial", Font.PLAIN, 24));
        mainPanel.add(milesField);

        // Convert button
        JButton convertButton = new JButton("Convert Distance");
        convertButton.setFont(new Font("Arial", Font.BOLD, 24));
        mainPanel.add(convertButton);

        // Results panel
        JPanel resultsPanel = new JPanel(new GridLayout(3, 1, 5, 5));
        kmResult = new JLabel("Kilometers: 0.00 km", JLabel.CENTER);
        metersResult = new JLabel("Meters: 0.00 m", JLabel.CENTER);
        feetResult = new JLabel("Feet: 0.00 ft", JLabel.CENTER);

        // Style result labels
        kmResult.setFont(new Font("Arial", Font.BOLD, 24));
        metersResult.setFont(new Font("Arial", Font.BOLD, 24));
        feetResult.setFont(new Font("Arial", Font.BOLD, 24));
        kmResult.setForeground(Color.BLUE);
        metersResult.setForeground(Color.BLUE);
        feetResult.setForeground(Color.BLUE);

        resultsPanel.add(kmResult);
        resultsPanel.add(metersResult);
        resultsPanel.add(feetResult);
        mainPanel.add(resultsPanel);

        // Add action listener to the button
        convertButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                convertDistance();
            }
        });

        // Also convert when Enter is pressed in the text field
        milesField.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                convertDistance();
            }
        });

        add(mainPanel);
    }

    private void convertDistance() {
        try {
            // Get the value from formatted text field
            double miles = ((Number) milesField.getValue()).doubleValue();

            // Perform conversions
            double kilometers = miles * 1.60934;
            double meters = kilometers * 1000;
            double feet = miles * 5280;

            // Update result labels with formatted numbers
            kmResult.setText(String.format("Kilometers: %.2f km", kilometers));
            metersResult.setText(String.format("Meters: %.2f m", meters));
            feetResult.setText(String.format("Feet: %.2f ft", feet));

        } catch (Exception e) {
            JOptionPane.showMessageDialog(this,
                    "Please enter a valid number for miles.",
                    "Input Error",
                    JOptionPane.ERROR_MESSAGE);
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new DistanceConverter().setVisible(true);
            }
        });
    }
}

```

## This is my result!

<img width="678" height="686" alt="image" src="https://github.com/user-attachments/assets/c9e985c1-d9e3-46b5-8dbf-9783ed8503be" />

<img width="674" height="661" alt="image" src="https://github.com/user-attachments/assets/70c64b04-851e-4d0e-a53d-9bf0d9558e5b" />

# VIDEO EXPLANATION!








