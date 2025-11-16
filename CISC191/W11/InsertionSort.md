# InsertionSort

### Armaan Shamsaasef
### Date: 11/16/2025
### CISC191 Week 11 Assignments


# Here is the flowchart!

<img width="2800" height="4769" alt="deepseek_mermaid_20251115_8eb035 (1)" src="https://github.com/user-attachments/assets/caf0cbef-86b8-4b9b-8125-8f3af510f3fd" />


## Explanation of my code.


## Challenges
Tracking Comparisons vs Swaps:

Initially confused about what counts as a "comparison" vs "swap"

Had to carefully analyze when to increment each counter

Array Index Management:

Off-by-one errors when shifting elements

Handling the j-- operation correctly in the while loop

Intermediate Outputs:

Deciding when to print the array (after each outer loop iteration)

Ensuring the output matches the expected format exactly

Input Parsing:

Handling the first number as array size vs array element

Properly converting string input to integers

## This is my code!

```.java

import java.util.Scanner;

public class InsertionSort {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Read input
        String[] input = scanner.nextLine().split(" ");
        int n = Integer.parseInt(input[0]);

        int[] arr = new int[n];
        for (int i = 0; i < n; i++) {
            arr[i] = Integer.parseInt(input[i + 1]);
        }

        // Output original array
        printArray(arr);

        // Perform insertion sort and track comparisons and swaps
        int comparisons = 0;
        int swaps = 0;

        for (int i = 1; i < n; i++) {
            int key = arr[i];
            int j = i - 1;

            // Move elements that are greater than key to one position ahead
            while (j >= 0) {
                comparisons++; // Count comparison
                if (arr[j] > key) {
                    arr[j + 1] = arr[j];
                    swaps++; // Count swap
                    j--;
                } else {
                    break;
                }
            }
            arr[j + 1] = key;

            // Output array after each iteration
            printArray(arr);
        }

        // Output final results
        System.out.println("comparisons: " + comparisons);
        System.out.println("swaps: " + swaps);

        scanner.close();
    }

    // Helper method to print array
    private static void printArray(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) {
                System.out.print(" ");
            }
        }
        System.out.println();
    }
}
```

## This is my result!



```.out
6 3 2 1 5 9 8



3 2 1 5 9 8
2 3 1 5 9 8
1 2 3 5 9 8
1 2 3 5 9 8
1 2 3 5 9 8
1 2 3 5 8 9
comparisons: 7
swaps: 4


```


# VIDEO EXPLANATION!

https://app.screencastify.com/watch/JcHoIOKJJ17EXzzP9hGU?checkOrg=30498c8e-8a1a-49cb-811f-fc81a0db6696






