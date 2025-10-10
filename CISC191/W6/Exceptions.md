# Exceptions

### Armaan Shamsaasef
### Date: 10/9/2025
### CISC Week 6 Assignments

# Flowchart

<img width="812" height="899" alt="image" src="https://github.com/user-attachments/assets/eb0327dd-2a60-4f94-969b-6d7daca938cc" />

# Explanation of my code.

Key Components:
stepsToMiles() Method:

Takes integer steps as parameter

Throws Exception if steps are negative

Returns miles as double (steps / 2000.0)

main() Method:

Uses Scanner to read user input

Implements try-catch block for exception handling

Calls stepsToMiles() within try block

Catches and displays exception messages

Uses printf for 2-decimal place formatting

Exception Handling:

Custom exception message for negative steps

Proper error message display

Graceful program termination

# Challenges I faced.
Design Phase Challenges:

Deciding how to split functionality between classes

Determining which class should handle the conversion logic

Designing clean interface between classes

Implementation Phase Challenges:

Ensuring proper method accessibility between classes

Maintaining the same functionality with separated classes

Handling Scanner and exception flow across classes


# Here is my code!

### StepConverter.java
```.java

/**
 * StepConverter class handles the conversion from steps to miles
 * and includes exception handling for negative step counts
 */
public class StepConverter {

    /**
     * Converts steps to miles (2000 steps = 1 mile)
     * @param steps the number of steps as integer
     * @return miles walked as double
     * @throws Exception when steps are negative
     */
    public static double stepsToMiles(int steps) throws Exception {
        // Check for negative step count
        if (steps < 0) {
            throw new Exception("Exception: Negative step count entered.");
        }

        // Calculate and return miles
        return steps / 2000.0;
    }
}
```


### StepCounterApp.java


```.java
import java.util.Scanner;

/**
 * StepCounterApp class handles user input and output
 * Runs continuously until user types 'exit' or 'quit'
 */
public class StepCounterApp {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("=== Step to Miles Converter ===");
        System.out.println("Enter number of steps to convert to miles");
        System.out.println("Type 'exit' or 'quit' to end the program");
        System.out.println("=================================");

        // Continuous loop for multiple conversions
        while (true) {
            try {
                System.out.print("\nEnter number of steps (or 'exit' to quit): ");
                String input = scanner.nextLine().trim();

                // Check for exit commands
                if (input.equalsIgnoreCase("exit") || input.equalsIgnoreCase("quit")) {
                    System.out.println("Thank you for using Step Converter. Goodbye!");
                    break;
                }

                // Convert input to integer
                int steps = Integer.parseInt(input);

                // Convert steps to miles using StepConverter class
                double miles = StepConverter.stepsToMiles(steps);

                // Output result with 2 decimal places
                System.out.printf("Miles walked: %.2f\n", miles);

            } catch (NumberFormatException e) {
                System.out.println("Error: Please enter a valid integer number or 'exit' to quit.");
            } catch (Exception e) {
                // Output exception message from StepConverter
                System.out.println(e.getMessage());
            }
        }

        scanner.close();
    }
}

```

# Output

```.java

=== Step to Miles Converter ===
Enter number of steps to convert to miles
Type 'exit' or 'quit' to end the program
=================================

Enter number of steps (or 'exit' to quit): 5345
Miles walked: 2.67

Enter number of steps (or 'exit' to quit): -3850
Exception: Negative step count entered.

Enter number of steps (or 'exit' to quit): abc
Error: Please enter a valid integer number or 'exit' to quit.

Enter number of steps (or 'exit' to quit): exit
Thank you for using Step Converter. Goodbye!

Process finished with exit code 0

```


## Video Explanation of Code!

https://app.screencastify.com/watch/KKOudhly7dnTKbWXve0d
