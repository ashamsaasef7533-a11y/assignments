# Miles to Kilometers Converter using JSpinner

### Armaan Shamsaasef
### Date: 11/9/2025
### CISC191 Week 10 Assignments


# Here is the flowchart!

<img width="301" height="936" alt="image" src="https://github.com/user-attachments/assets/9dc5143d-e994-4afd-a683-a7250a8367f7" />


## Explanation of my code.

This Java program creates a simple distance converter that uses a JSpinner for input instead of a text field. The spinner allows users to select mile values using up/down arrows or by typing. When the user clicks the convert button or changes the spinner value, the program multiplies the miles by 1.60934 to calculate kilometers and displays the result in a prominently styled label. The interface is clean and intuitive, providing immediate feedback for each conversion.

## Challenges

JSpinner Configuration: Setting up the SpinnerNumberModel with appropriate minimum, maximum, and step values required careful parameter selection

Instant Conversion: Implementing both button click and spinner change listeners to provide flexible user interaction

Value Retrieval: Properly casting the spinner value to Double and handling potential ClassCastException

Layout Balance: Arranging the spinner, button, and result label in a visually balanced layout

User Experience: Ensuring the spinner arrows are easy to use and the conversion happens smoothly with both interaction methods

## This is my code!

```.java

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class MilesToKmConverter extends JFrame {
    private JSpinner milesSpinner;
    private JLabel kmResult;

    public MilesToKmConverter() {
        // Set up the main window
        setTitle("Miles to Kilometers Converter");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(350, 200);
        setLocationRelativeTo(null);

        // Create main panel
        JPanel mainPanel = new JPanel();
        mainPanel.setLayout(new GridLayout(4, 1, 15, 15));
        mainPanel.setBorder(BorderFactory.createEmptyBorder(20, 20, 20, 20));

        // Title
        JLabel titleLabel = new JLabel("Miles to Kilometers Converter", JLabel.CENTER);
        titleLabel.setFont(new Font("Arial", Font.BOLD, 18));
        mainPanel.add(titleLabel);

        // Input panel with spinner
        JPanel inputPanel = new JPanel(new FlowLayout());
        JLabel inputLabel = new JLabel("Distance in Miles:");
        inputLabel.setFont(new Font("Arial", Font.PLAIN, 14));

        // Create JSpinner with number model
        SpinnerNumberModel spinnerModel = new SpinnerNumberModel(0.0, 0.0, 10000.0, 0.1);
        milesSpinner = new JSpinner(spinnerModel);
        milesSpinner.setPreferredSize(new Dimension(100, 30));
        milesSpinner.setFont(new Font("Arial", Font.PLAIN, 14));

        inputPanel.add(inputLabel);
        inputPanel.add(milesSpinner);
        mainPanel.add(inputPanel);

        // Convert button
        JButton convertButton = new JButton("Convert to Kilometers");
        convertButton.setFont(new Font("Arial", Font.BOLD, 14));
        mainPanel.add(convertButton);

        // Result label
        kmResult = new JLabel("Kilometers: 0.00 km", JLabel.CENTER);
        kmResult.setFont(new Font("Arial", Font.BOLD, 16));
        kmResult.setForeground(Color.BLUE);
        kmResult.setBorder(BorderFactory.createLineBorder(Color.BLACK, 1));
        kmResult.setOpaque(true);
        kmResult.setBackground(Color.WHITE);
        mainPanel.add(kmResult);

        // Add action listener to the button
        convertButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                convertMilesToKm();
            }
        });

        // Also add change listener to spinner for instant conversion
        milesSpinner.addChangeListener(e -> convertMilesToKm());

        add(mainPanel);
    }

    private void convertMilesToKm() {
        try {
            // Get the value from spinner
            double miles = ((Double) milesSpinner.getValue());

            // Convert miles to kilometers
            double kilometers = miles * 1.60934;

            // Update result label with formatted number
            kmResult.setText(String.format("Kilometers: %.2f km", kilometers));

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
                new MilesToKmConverter().setVisible(true);
            }
        });
    }
}

```

## This is my result!

<img width="577" height="440" alt="image" src="https://github.com/user-attachments/assets/79dd2a2a-58b6-4e02-9b1a-766c0dc69b89" />


<img width="578" height="436" alt="image" src="https://github.com/user-attachments/assets/c7884659-b78b-4cfb-8229-6a8321f3d783" />



# VIDEO EXPLANATION!








