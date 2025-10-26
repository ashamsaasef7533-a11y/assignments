# Collections

### Armaan Shamsaasef
### Date: 10/21/2025
### CISC Week 7 Assignments

# Flowchart
<img width="330" height="639" alt="image" src="https://github.com/user-attachments/assets/bf079e20-d792-4f72-b6bd-6e6a06e3f051" />


# Explanation of my code.

Input Processing: Reads a string and converts it to lowercase for case-insensitive comparison

Character Filtering: Uses Character.isLetterOrDigit() to ignore punctuation and spaces

Deque Operations:

Adds filtered characters to the deque using addLast()

Compares characters from both ends using removeFirst() and removeLast()

Palindrome Check: Continues comparing until the deque has 0 or 1 character left

Result Display: Outputs appropriate message based on the palindrome check

Key Features:

Handles punctuation and spaces by ignoring them

Works with mixed-case input by converting to lowercase

Efficient O(n) time complexity

Handles edge cases like single characters and empty strings

Uses Java's Collections framework effectively with Deque


# Challenges I faced.
#### Design Phase Challenges:
Understanding Deque operations: Determining whether to use ArrayDeque or LinkedList implementation

Character filtering: Deciding how to handle punctuation and spaces while maintaining efficiency

Edge cases: Considering empty strings, single characters, and strings with only punctuation

#### Implementation Phase Challenges:
Auto-boxing: Remembering that primitive char automatically converts to Character when added to Deque

Efficient comparison: Implementing the two-pointer approach using deque operations

Input handling: Ensuring the program works with various input types and handles unexpected inputs gracefully

Resource management: Properly closing the Scanner to prevent resource leaks

# Here is my code!

### PalindromeChecker.java
```.java

import java.util.Deque;
import java.util.ArrayDeque;
import java.util.Scanner;

public class PalindromeChecker {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("=== Palindrome Checker ===");
        System.out.println("Check multiple strings for palindromes");
        System.out.println("Type 'exit' or 'quit' to end the program");
        System.out.println("----------------------------------------");

        while (true) {
            System.out.print("\nEnter a string to check (or 'exit' to quit): ");
            String input = scanner.nextLine().trim();

            // Check for exit conditions
            if (input.equalsIgnoreCase("exit") ||
                    input.equalsIgnoreCase("quit") ||
                    input.equalsIgnoreCase("q")) {
                System.out.println("Thank you for using Palindrome Checker. Goodbye!");
                break;
            }

            // Check for empty input
            if (input.isEmpty()) {
                System.out.println("Please enter a valid string.");
                continue;
            }

            // Process the input
            String result = isPalindrome(input) ? "Yes" : "No";
            String message = result + ", \"" + input + "\" is " +
                    (isPalindrome(input) ? "" : "not ") + "a palindrome.";

            System.out.println(message);

            // Show additional info for educational purposes
            showDequeProcess(input);
        }

        scanner.close();
    }

    public static boolean isPalindrome(String str) {
        // Handle empty string and single character cases
        if (str == null || str.length() <= 1) {
            return true;
        }

        // Create a deque to store characters
        Deque<Character> deque = new ArrayDeque<>();

        // Filter and add only alphanumeric characters to the deque
        for (char c : str.toCharArray()) {
            if (Character.isLetterOrDigit(c)) {
                deque.addLast(Character.toLowerCase(c));
            }
        }

        // Check if the filtered string is a palindrome
        while (deque.size() > 1) {
            char first = deque.removeFirst();
            char last = deque.removeLast();

            if (first != last) {
                return false;
            }
        }

        return true;
    }

    // Additional method to demonstrate how the deque processes the input
    private static void showDequeProcess(String str) {
        Deque<Character> deque = new ArrayDeque<>();
        StringBuilder filtered = new StringBuilder();

        // Filter the string
        for (char c : str.toCharArray()) {
            if (Character.isLetterOrDigit(c)) {
                deque.addLast(Character.toLowerCase(c));
                filtered.append(Character.toLowerCase(c));
            }
        }

        System.out.println("  Processed as: \"" + filtered.toString() + "\"");
        System.out.println("  Characters in deque: " + deque.size());

        if (deque.size() <= 10) { // Only show small strings for clarity
            System.out.println("  Deque contents: " + deque);
        }
    }
}
```

# Output

```.java

=== Palindrome Checker ===
Check multiple strings for palindromes
Type 'exit' or 'quit' to end the program
----------------------------------------

Enter a string to check (or 'exit' to quit): civic
Yes, "civic" is a palindrome.
  Processed as: "civic"
  Characters in deque: 5
  Deque contents: [c, i, v, i, c]

Enter a string to check (or 'exit' to quit): rotostor
No, "rotostor" is not a palindrome.
  Processed as: "rotostor"
  Characters in deque: 8
  Deque contents: [r, o, t, o, s, t, o, r]

Enter a string to check (or 'exit' to quit): exit
Thank you for using Palindrome Checker. Goodbye!

Process finished with exit code 0

```


## Video Explanation of Code!

https://app.screencastify.com/watch/tmRphFDHIu2TqhxkgQG4 
