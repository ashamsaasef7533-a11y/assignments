
# Armaan Shamsaasef--CISC191

## Code

### Bandages.java

```.java
// Derived class for Bandages
public class Bandage extends Inventory {
    private String expiryDate;
    private String ageGroup;
    private char waterProof;

    public Bandage(String name, String companyName, double itemWeight,
                   String expiryDate, String ageGroup, char waterProof) {
        super(name, companyName, itemWeight);
        this.expiryDate = expiryDate;
        this.ageGroup = ageGroup;
        this.waterProof = waterProof;
    }

    // Override display method
    @Override
    public void display() {
        String waterproofStatus = (waterProof == 'Y' || waterProof == 'y') ? "Yes" : "No";
        System.out.println("BANDAGE - Item: " + name + ", Company: " + companyName +
                ", Weight: " + itemWeight + " lbs, Expiry: " + expiryDate +
                ", Age Group: " + ageGroup + ", Waterproof: " + waterproofStatus);
    }

    // Overloaded update method specific to Bandages
    public void update(String name, String companyName, double itemWeight,
                       String expiryDate, String ageGroup, char waterProof) {
        super.update(name, companyName, itemWeight);
        this.expiryDate = expiryDate;
        this.ageGroup = ageGroup;
        this.waterProof = waterProof;
    }
} 
```

### EmptyFieldException.java

```.java
// Custom Exception Class for Empty Field Validation
public class EmptyFieldException extends Exception {
    public EmptyFieldException(String message) {
        super(message);
    }
}
```

### Equipment.java

```.java
// Derived class for Equipment
public class Equipment extends Inventory {
    private String category;
    private double itemWeight;

    public Equipment(String name, String companyName, double itemWeight, String category) {
        super(name, companyName, itemWeight);
        this.category = category;
    }

    // Override display method
    @Override
    public void display() {
        System.out.println("EQUIPMENT - Item: " + name + ", Company: " + companyName +
                ", Weight: " + itemWeight + " lbs, Category: " + category);
    }

    // Overloaded update method specific to Equipment
    public void update(String name, String companyName, double itemWeight, String category) {
        super.update(name, companyName, itemWeight);
        this.category = category;
    }
}

```


### InvalidWeightException.java


```.java

// Custom Exception Class for Weight Validation
public class InvalidWeightException extends Exception {
    public InvalidWeightException(String message) {
        super(message);
    }
}



```

### Inventorty.java

```.java

// Parent class
public class Inventory {
    protected String name;
    protected String companyName;
    protected double itemWeight;

    public Inventory(String name, String companyName, double itemWeight) {
        this.name = name;
        this.companyName = companyName;
        this.itemWeight = itemWeight;
    }

    // Display method - will be overridden in derived classes
    public void display() {
        System.out.println("Item: " + name + ", Company: " + companyName +
                ", Weight: " + itemWeight + " lbs");
    }

    // Update method - overloaded versions available
    public void update(String name, String companyName, double itemWeight) {
        this.name = name;
        this.companyName = companyName;
        this.itemWeight = itemWeight;
    }

    // Overloaded update method
    public void update(String name, String companyName) {
        this.name = name;
        this.companyName = companyName;
    }

    // Getters
    public String getName() { return name; }
    public String getCompanyName() { return companyName; }
    public double getItemWeight() { return itemWeight; }
}


```

### PainKiller.java


```.java

// Derived class for Painkillers
public class PainKiller extends Inventory {
    private String expiryDate;
    private String ageGroup;

    public PainKiller(String name, String companyName, double itemWeight,
                      String expiryDate, String ageGroup) {
        super(name, companyName, itemWeight);
        this.expiryDate = expiryDate;
        this.ageGroup = ageGroup;
    }

    // Override display method
    @Override
    public void display() {
        System.out.println("PAINKILLER - Item: " + name + ", Company: " + companyName +
                ", Weight: " + itemWeight + " lbs, Expiry: " + expiryDate +
                ", Age Group: " + ageGroup);
    }

    // Overloaded update method specific to Painkillers
    public void update(String name, String companyName, double itemWeight,
                       String expiryDate, String ageGroup) {
        super.update(name, companyName, itemWeight);
        this.expiryDate = expiryDate;
        this.ageGroup = ageGroup;
    }
}


```





# Version 1
### InventoryControlSystemV1.java

```.java

import java.util.*;

// Inventory Control System Version 1.0
public class InventoryControlSystemV1 {
    private Scanner scanner;
    private List<Inventory> inventory;

    public InventoryControlSystemV1() {
        scanner = new Scanner(System.in);
        inventory = new ArrayList<>();
    }

    public void run() {
        System.out.println("=== Inventory Control System Version 1.0 ===");

        while (true) {
            displayMenu();
            int choice = getIntInput("Enter your choice: ");

            switch (choice) {
                case 1:
                    addItem();
                    break;
                case 2:
                    displayAllItems();
                    break;
                case 3:
                    updateItem();
                    break;
                case 4:
                    System.out.println("Exiting system...");
                    return;
                default:
                    System.out.println("Invalid choice! Please try again.");
            }
        }
    }

    private void displayMenu() {
        System.out.println("\n--- Main Menu ---");
        System.out.println("1. Add Item");
        System.out.println("2. Display All Items");
        System.out.println("3. Update Item");
        System.out.println("4. Exit");
    }

    private void addItem() {
        System.out.println("\n--- Add New Item ---");
        System.out.println("1. Painkiller");
        System.out.println("2. Bandage");
        System.out.println("3. Equipment");

        int type = getIntInput("Select item type: ");

        System.out.print("Enter item name: ");
        String name = scanner.nextLine();
        System.out.print("Enter company name: ");
        String company = scanner.nextLine();
        double weight = getDoubleInput("Enter item weight (lbs): ");

        switch (type) {
            case 1: // PainKiller
                System.out.print("Enter expiry date: ");
                String expiry = scanner.nextLine();
                System.out.print("Enter age group: ");
                String ageGroup = scanner.nextLine();
                inventory.add(new PainKiller(name, company, weight, expiry, ageGroup));
                break;
            case 2: // Bandage
                System.out.print("Enter expiry date: ");
                expiry = scanner.nextLine();
                System.out.print("Enter age group: ");
                ageGroup = scanner.nextLine();
                System.out.print("Is waterproof? (Y/N): ");
                char waterproof = scanner.nextLine().charAt(0);
                inventory.add(new Bandage(name, company, weight, expiry, ageGroup, waterproof));
                break;
            case 3: // Equipment
                System.out.print("Enter equipment category: ");
                String category = scanner.nextLine();
                inventory.add(new Equipment(name, company, weight, category));
                break;
            default:
                System.out.println("Invalid item type!");
                return;
        }
        System.out.println("Item added successfully!");
    }

    private void displayAllItems() {
        System.out.println("\n--- All Inventory Items ---");
        if (inventory.isEmpty()) {
            System.out.println("No items in inventory.");
            return;
        }

        for (int i = 0; i < inventory.size(); i++) {
            System.out.print((i + 1) + ". ");
            inventory.get(i).display();
        }
    }

    private void updateItem() {
        if (inventory.isEmpty()) {
            System.out.println("No items to update.");
            return;
        }

        displayAllItems();
        int index = getIntInput("Enter item number to update: ") - 1;

        if (index < 0 || index >= inventory.size()) {
            System.out.println("Invalid item number!");
            return;
        }

        Inventory item = inventory.get(index);
        System.out.print("Enter new name (current: " + item.getName() + "): ");
        String name = scanner.nextLine();
        System.out.print("Enter new company name (current: " + item.getCompanyName() + "): ");
        String company = scanner.nextLine();
        double weight = getDoubleInput("Enter new weight (current: " + item.getItemWeight() + "): ");

        item.update(name, company, weight);
        System.out.println("Item updated successfully!");
    }

    private int getIntInput(String prompt) {
        System.out.print(prompt);
        while (!scanner.hasNextInt()) {
            System.out.println("Please enter a valid number!");
            scanner.next();
            System.out.print(prompt);
        }
        int value = scanner.nextInt();
        scanner.nextLine(); // consume newline
        return value;
    }

    private double getDoubleInput(String prompt) {
        System.out.print(prompt);
        while (!scanner.hasNextDouble()) {
            System.out.println("Please enter a valid number!");
            scanner.next();
            System.out.print(prompt);
        }
        double value = scanner.nextDouble();
        scanner.nextLine(); // consume newline
        return value;
    }

    public static void main(String[] args) {
        InventoryControlSystemV1 system = new InventoryControlSystemV1();
        system.run();
    }
}


```

# Version 2

### InventoryControlSystemV2.java

```.java

import java.util.*;
import java.util.InputMismatchException;

public class InventoryControlSystemV2 {
    private Scanner scanner;
    private List<Inventory> inventory;

    public InventoryControlSystemV2() {
        scanner = new Scanner(System.in);
        inventory = new ArrayList<>();
    }

    public void run() {
        System.out.println("=== Inventory Control System Version 2.0 ===");
        System.out.println("Now with exception handling!");

        while (true) {
            try {
                displayMenu();
                int choice = getIntInputWithException("Enter your choice: ");

                switch (choice) {
                    case 1:
                        addItemWithExceptionHandling();
                        break;
                    case 2:
                        displayAllItems();
                        break;
                    case 3:
                        updateItemWithExceptionHandling();
                        break;
                    case 4:
                        System.out.println("Exiting system...");
                        return;
                    default:
                        System.out.println("Invalid choice! Please try again.");
                }
            } catch (Exception e) {
                System.out.println("Error: " + e.getMessage());
            }
        }
    }

    private void displayMenu() {
        System.out.println("\n--- Main Menu ---");
        System.out.println("1. Add Item");
        System.out.println("2. Display All Items");
        System.out.println("3. Update Item");
        System.out.println("4. Exit");
    }

    private void addItemWithExceptionHandling() {
        try {
            System.out.println("\n--- Add New Item ---");
            System.out.println("1. PainKiller");
            System.out.println("2. Bandage");
            System.out.println("3. Equipment");

            int type = getIntInputWithException("Select item type: ");

            // InputMismatchException handling for string inputs
            System.out.print("Enter item name: ");
            String name = scanner.nextLine();
            if (name.trim().isEmpty()) {
                throw new IllegalArgumentException("Item name cannot be empty!");
            }

            System.out.print("Enter company name: ");
            String company = scanner.nextLine();
            if (company.trim().isEmpty()) {
                throw new IllegalArgumentException("Company name cannot be empty!");
            }

            double weight = getDoubleInputWithException("Enter item weight (lbs): ");
            if (weight <= 0) {
                throw new IllegalArgumentException("Weight must be positive!");
            }

            switch (type) {
                case 1: // PainKiller
                    System.out.print("Enter expiry date: ");
                    String expiry = scanner.nextLine();
                    System.out.print("Enter age group: ");
                    String ageGroup = scanner.nextLine();
                    inventory.add(new PainKiller(name, company, weight, expiry, ageGroup));
                    break;
                case 2: // Bandage
                    System.out.print("Enter expiry date: ");
                    expiry = scanner.nextLine();
                    System.out.print("Enter age group: ");
                    ageGroup = scanner.nextLine();
                    System.out.print("Is waterproof? (Y/N): ");
                    char waterproof = scanner.nextLine().charAt(0);
                    inventory.add(new Bandage(name, company, weight, expiry, ageGroup, waterproof));
                    break;
                case 3: // Equipment
                    System.out.print("Enter equipment category: ");
                    String category = scanner.nextLine();
                    inventory.add(new Equipment(name, company, weight, category));
                    break;
                default:
                    System.out.println("Invalid item type!");
                    return;
            }
            System.out.println("Item added successfully!");

        } catch (InputMismatchException e) {
            System.out.println("Error: Invalid input type! Please enter the correct data type.");
            scanner.nextLine(); // clear invalid input
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        } catch (Exception e) {
            System.out.println("Unexpected error: " + e.getMessage());
        }
    }

    private void displayAllItems() {
        System.out.println("\n--- All Inventory Items ---");
        if (inventory.isEmpty()) {
            System.out.println("No items in inventory.");
            return;
        }

        for (int i = 0; i < inventory.size(); i++) {
            System.out.print((i + 1) + ". ");
            inventory.get(i).display();
        }
    }

    private void updateItemWithExceptionHandling() {
        try {
            if (inventory.isEmpty()) {
                System.out.println("No items to update.");
                return;
            }

            displayAllItems();
            int index = getIntInputWithException("Enter item number to update: ") - 1;

            if (index < 0 || index >= inventory.size()) {
                throw new IndexOutOfBoundsException("Invalid item number!");
            }

            Inventory item = inventory.get(index);
            System.out.print("Enter new name (current: " + item.getName() + "): ");
            String name = scanner.nextLine();
            if (name.trim().isEmpty()) {
                throw new IllegalArgumentException("Item name cannot be empty!");
            }

            System.out.print("Enter new company name (current: " + item.getCompanyName() + "): ");
            String company = scanner.nextLine();
            if (company.trim().isEmpty()) {
                throw new IllegalArgumentException("Company name cannot be empty!");
            }

            double weight = getDoubleInputWithException("Enter new weight (current: " + item.getItemWeight() + "): ");
            if (weight <= 0) {
                throw new IllegalArgumentException("Weight must be positive!");
            }

            item.update(name, company, weight);
            System.out.println("Item updated successfully!");

        } catch (InputMismatchException e) {
            System.out.println("Error: Please enter a valid number!");
            scanner.nextLine();
        } catch (IndexOutOfBoundsException | IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }

    private int getIntInputWithException(String prompt) {
        System.out.print(prompt);
        try {
            int value = scanner.nextInt();
            scanner.nextLine(); // consume newline
            return value;
        } catch (InputMismatchException e) {
            scanner.nextLine(); // clear invalid input
            throw new InputMismatchException("Invalid integer input!");
        }
    }

    private double getDoubleInputWithException(String prompt) {
        System.out.print(prompt);
        try {
            double value = scanner.nextDouble();
            scanner.nextLine(); // consume newline
            return value;
        } catch (InputMismatchException e) {
            scanner.nextLine(); // clear invalid input
            throw new InputMismatchException("Invalid number input!");
        }
    }

    public static void main(String[] args) {
        InventoryControlSystemV2 system = new InventoryControlSystemV2();
        system.run();
    }
}


```
# Version 3
### InventoryControlSystemV3.java

```.java

import java.util.*;

public class InventoryControlSystemV3 {
    private Scanner scanner;
    private List<Inventory> inventory;

    public InventoryControlSystemV3() {
        scanner = new Scanner(System.in);
        inventory = new ArrayList<>();
    }

    public void run() {
        System.out.println("=== Inventory Control System Version 3.0 ===");
        System.out.println("Now with custom exception handling!");

        while (true) {
            try {
                displayMenu();
                int choice = getIntInput("Enter your choice: ");

                switch (choice) {
                    case 1:
                        addItemWithCustomExceptions();
                        break;
                    case 2:
                        displayAllItems();
                        break;
                    case 3:
                        updateItemWithCustomExceptions();
                        break;
                    case 4:
                        System.out.println("Exiting system...");
                        return;
                    default:
                        System.out.println("Invalid choice! Please try again.");
                }
            } catch (Exception e) {
                System.out.println("Error: " + e.getMessage());
            }
        }
    }

    private void displayMenu() {
        System.out.println("\n--- Main Menu ---");
        System.out.println("1. Add Item");
        System.out.println("2. Display All Items");
        System.out.println("3. Update Item");
        System.out.println("4. Exit");
    }

    private void addItemWithCustomExceptions() {
        try {
            System.out.println("\n--- Add New Item ---");
            System.out.println("1. PainKiller");
            System.out.println("2. Bandage");
            System.out.println("3. Equipment");

            int type = getIntInput("Select item type: ");

            // Using custom exception for empty field validation
            System.out.print("Enter item name: ");
            String name = scanner.nextLine();
            if (name.trim().isEmpty()) {
                throw new EmptyFieldException("Item name cannot be empty!");
            }

            System.out.print("Enter company name: ");
            String company = scanner.nextLine();
            if (company.trim().isEmpty()) {
                throw new EmptyFieldException("Company name cannot be empty!");
            }

            // Using custom exception for weight validation
            double weight = getDoubleInput("Enter item weight (lbs): ");
            if (weight <= 0) {
                throw new InvalidWeightException("Weight must be a positive number!");
            }
            if (weight > 1000) {
                throw new InvalidWeightException("Weight cannot exceed 1000 lbs!");
            }

            switch (type) {
                case 1: // PainKiller
                    System.out.print("Enter expiry date: ");
                    String expiry = scanner.nextLine();
                    if (expiry.trim().isEmpty()) {
                        throw new EmptyFieldException("Expiry date cannot be empty!");
                    }
                    System.out.print("Enter age group: ");
                    String ageGroup = scanner.nextLine();
                    if (ageGroup.trim().isEmpty()) {
                        throw new EmptyFieldException("Age group cannot be empty!");
                    }
                    inventory.add(new PainKiller(name, company, weight, expiry, ageGroup));
                    break;
                case 2: // Bandage
                    System.out.print("Enter expiry date: ");
                    expiry = scanner.nextLine();
                    if (expiry.trim().isEmpty()) {
                        throw new EmptyFieldException("Expiry date cannot be empty!");
                    }
                    System.out.print("Enter age group: ");
                    ageGroup = scanner.nextLine();
                    if (ageGroup.trim().isEmpty()) {
                        throw new EmptyFieldException("Age group cannot be empty!");
                    }
                    System.out.print("Is waterproof? (Y/N): ");
                    char waterproof = scanner.nextLine().charAt(0);
                    inventory.add(new Bandage(name, company, weight, expiry, ageGroup, waterproof));
                    break;
                case 3: // Equipment
                    System.out.print("Enter equipment category: ");
                    String category = scanner.nextLine();
                    if (category.trim().isEmpty()) {
                        throw new EmptyFieldException("Category cannot be empty!");
                    }
                    inventory.add(new Equipment(name, company, weight, category));
                    break;
                default:
                    System.out.println("Invalid item type!");
                    return;
            }
            System.out.println("Item added successfully!");

        } catch (InvalidWeightException | EmptyFieldException e) {
            System.out.println("Validation Error: " + e.getMessage());
        } catch (InputMismatchException e) {
            System.out.println("Error: Invalid input type! Please enter the correct data type.");
            scanner.nextLine();
        } catch (Exception e) {
            System.out.println("Unexpected error: " + e.getMessage());
        }
    }

    private void displayAllItems() {
        System.out.println("\n--- All Inventory Items ---");
        if (inventory.isEmpty()) {
            System.out.println("No items in inventory.");
            return;
        }

        for (int i = 0; i < inventory.size(); i++) {
            System.out.print((i + 1) + ". ");
            inventory.get(i).display();
        }
    }

    private void updateItemWithCustomExceptions() {
        try {
            if (inventory.isEmpty()) {
                System.out.println("No items to update.");
                return;
            }

            displayAllItems();
            int index = getIntInput("Enter item number to update: ") - 1;

            if (index < 0 || index >= inventory.size()) {
                throw new IndexOutOfBoundsException("Invalid item number!");
            }

            Inventory item = inventory.get(index);
            System.out.print("Enter new name (current: " + item.getName() + "): ");
            String name = scanner.nextLine();
            if (name.trim().isEmpty()) {
                throw new EmptyFieldException("Item name cannot be empty!");
            }

            System.out.print("Enter new company name (current: " + item.getCompanyName() + "): ");
            String company = scanner.nextLine();
            if (company.trim().isEmpty()) {
                throw new EmptyFieldException("Company name cannot be empty!");
            }

            double weight = getDoubleInput("Enter new weight (current: " + item.getItemWeight() + "): ");
            if (weight <= 0) {
                throw new InvalidWeightException("Weight must be a positive number!");
            }
            if (weight > 1000) {
                throw new InvalidWeightException("Weight cannot exceed 1000 lbs!");
            }

            item.update(name, company, weight);
            System.out.println("Item updated successfully!");

        } catch (InvalidWeightException | EmptyFieldException e) {
            System.out.println("Validation Error: " + e.getMessage());
        } catch (InputMismatchException e) {
            System.out.println("Error: Please enter a valid number!");
            scanner.nextLine();
        } catch (IndexOutOfBoundsException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }

    private int getIntInput(String prompt) {
        System.out.print(prompt);
        while (!scanner.hasNextInt()) {
            System.out.println("Please enter a valid number!");
            scanner.next();
            System.out.print(prompt);
        }
        int value = scanner.nextInt();
        scanner.nextLine(); // consume newline
        return value;
    }

    private double getDoubleInput(String prompt) {
        System.out.print(prompt);
        while (!scanner.hasNextDouble()) {
            System.out.println("Please enter a valid number!");
            scanner.next();
            System.out.print(prompt);
        }
        double value = scanner.nextDouble();
        scanner.nextLine(); // consume newline
        return value;
    }

    public static void main(String[] args) {
        InventoryControlSystemV3 system = new InventoryControlSystemV3();
        system.run();
    }
}


```


