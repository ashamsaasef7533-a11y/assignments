# Salary Calculator

### Armaan Shamsaasef
### 9/6/2025
### CISC Week 2 Assignments

# Flowchart




# Explanation of my code.





# Challenges I faced.



# Here is my code!
```.java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scnr = new Scanner(System.in);
        TaxTableTools table = new TaxTableTools();
        int annualSalary;
        double taxRate;
        int taxToPay;

        // Set new salary and tax rate table using the setter method
        int[] newSalary = {0, 20000, 50000, 100000, Integer.MAX_VALUE};
        double[] newRate = {0.0, 0.10, 0.20, 0.30, 0.40};
        table.setTaxTable(newSalary, newRate);

        System.out.println("\nEnter annual salary (-1 to exit): ");
        annualSalary = scnr.nextInt();

        while (annualSalary >= 0) {
            taxRate = table.getValue(annualSalary);
            taxToPay = (int)(annualSalary * taxRate);  // Truncate tax to an integer amount
            System.out.println("Annual Salary: " + annualSalary +
                    "\tTax rate: " + taxRate +
                    "\tTax to pay: " + taxToPay);

            // Get next annual salary
            System.out.println("\nEnter annual salary (-1 to exit): ");
            annualSalary = scnr.nextInt();
        }
    }
}

class TaxTableTools {
    private int[] salary = {   0,  20000,  50000, 100000, Integer.MAX_VALUE };
    private double[] rate = { 0.0,   0.10,   0.20,   0.30,              0.40 };

    // Setter method to accept new salary and tax rate table
    public void setTaxTable(int[] newSalary, double[] newRate) {
        if (newSalary.length != newRate.length) {
            System.out.println("Error: Salary and rate arrays must be the same length");
            return;
        }
        this.salary = newSalary;
        this.rate = newRate;
    }

    public double getValue(int searchSalary) {
        int i;
        double taxRate;

        taxRate = 0.0;

        for (i = 0; i < salary.length; ++i) {
            if (searchSalary <= salary[i]) {
                taxRate = rate[i];
                return taxRate;
            }
        }

        return taxRate;
    }
}

```



# Output

```.java

Enter annual salary (-1 to exit): 
1111111
Annual Salary: 1111111	Tax rate: 0.4	Tax to pay: 444444

Enter annual salary (-1 to exit): 
-1

Process finished with exit code 0





```


## Video Explanation of Code!

