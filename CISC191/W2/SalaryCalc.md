# Salary Calculator

### Armaan Shamsaasef
### 9/6/2025
### CISC Week 2 Assignments

# Flowchart

<img width="424" height="936" alt="image" src="https://github.com/user-attachments/assets/d7087793-510c-4345-87f1-b2df87c4597b" />



# Explanation of my code.

1. Program Setup
        The program creates a "tax table" with different income levels and their corresponding tax rates

2. User Interaction
        The program asks you to enter your annual salary
        You type in your salary amount (like 35000 or 75000)
        If you enter -1, the program ends

3. Tax Calculation Process
        The program takes your salary and compares it to the tax table
        It finds which tax bracket your salary falls into
        It calculates how much tax you owe based on that bracket's rate


4. Example Calculations


         Salary: {   0,  20000, 50000, 100000, Integer.MAX_VALUE }
         Tax Rate: { 0.0, 0.10,  0.20,   0.30,            0.40   }
        
        Based on the above table we calculate the tax:
        For example:
        
        Anything greater then 100000 would be taxed 40%.


6. Display Results
        The program shows you:
        Your salary, 
        Your tax rate, 
        The actual tax amount you need to pay

# Challenges I faced.

The main challenges I encountered were:

Understanding the relationship between classes: Ensuring proper communication between the main class and TaxTableTools class required careful planning.

Constructor overloading: Making sure both constructors worked correctly and didn't interfere with each other.

Table management: Ensuring the setter method and overloaded constructor properly handled the arrays and maintained the nEntries value.

Edge cases: Testing with boundary values like Integer.MAX_VALUE and negative salaries to ensure robust behavior.

Code organization: Keeping the code clean and readable while implementing all required functionality.


# Here is my code!

### TaxTableTools.java

```.java
public class TaxTableTools {

    private int [] search =   {   0,  20000, 50000, 100000, Integer.MAX_VALUE };
    private double [] value = { 0.0,   0.10,  0.20,   0.30,              0.40 };
    private int nEntries;

    // Default constructor
    public TaxTableTools () {
        nEntries  = search.length;
    }

    // Setter method to set new tables
    public void setTables(int[] newSearch, double[] newValue) {
        search = newSearch;
        value = newValue;
        nEntries = search.length;
    }

    public double getValue(int searchArgument) {
        double result = 0.0;
        boolean keepLooking = true;
        int i = 0;

        while ((i < nEntries) && keepLooking) {
            if (searchArgument <= search[i]) {
                result = value[i];
                keepLooking = false;
            }
            else {
                ++i;
            }
        }

        return result;
    }
}


```
### IncomeTaxMain.java

```.java
import java.util.Scanner;

public class IncomeTaxMain {
    // Method to prompt for and input an integer
    public static int getInteger(Scanner input, String prompt) {
        int inputValue;

        System.out.print(prompt + ": ");
        inputValue = input.nextInt();

        return inputValue;
    }

    public static void main(String [] args) {
        final String PROMPT_SALARY = "\nEnter annual salary (-1 to exit)";
        Scanner scnr = new Scanner(System.in);
        int annualSalary;
        double taxRate;
        int taxToPay;

        int []    salary   = {   0,  20000, 50000, 100000, Integer.MAX_VALUE };
        double [] taxTable = { 0.0,   0.10,  0.20,   0.30,              0.40 };

        TaxTableTools table = new TaxTableTools();

        // Call setter method to supply new tables
        table.setTables(salary, taxTable);

        annualSalary = getInteger(scnr, PROMPT_SALARY);

        while (annualSalary >= 0) {
            taxRate = table.getValue(annualSalary);
            taxToPay = (int)(annualSalary * taxRate);
            System.out.println("Annual Salary: " + annualSalary +
                    "\tTax rate: " + taxRate +
                    "\tTax to pay: " + taxToPay);

            annualSalary = getInteger(scnr, PROMPT_SALARY);
        }

        System.out.println("Program ended.");
        scnr.close();
    }
}


```


# Output

```.java

Enter annual salary (-1 to exit): 275000
Annual Salary: 275000	Tax rate: 0.4	Tax to pay: 110000

Enter annual salary (-1 to exit): 100000
Annual Salary: 100000	Tax rate: 0.3	Tax to pay: 30000

Enter annual salary (-1 to exit): 30000
Annual Salary: 30000	Tax rate: 0.2	Tax to pay: 6000

Enter annual salary (-1 to exit): 15000
Annual Salary: 15000	Tax rate: 0.1	Tax to pay: 1500

Enter annual salary (-1 to exit): 7500
Annual Salary: 7500	Tax rate: 0.1	Tax to pay: 750

Enter annual salary (-1 to exit): 5000
Annual Salary: 5000	Tax rate: 0.1	Tax to pay: 500

Enter annual salary (-1 to exit): 0
Annual Salary: 0	Tax rate: 0.0	Tax to pay: 0

Enter annual salary (-1 to exit): -1
Program ended.

Process finished with exit code 0

```


## Video Explanation of Code!

