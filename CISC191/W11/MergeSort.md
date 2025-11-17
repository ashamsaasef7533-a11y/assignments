# MergeSort

### Armaan Shamsaasef
### Date: 11/16/2025
### CISC191 Week 11 Assignments


# Here is the flowchart!


<img width="333" height="940" alt="image" src="https://github.com/user-attachments/assets/193614a3-bc91-4240-9011-99da3ff897f3" />


## Explanation of my code.

The code reads the input, performs merge sort while counting comparisons and swaps, and outputs the results in the required format. For the input 6 3 2 1 5 9 8, it produces the expected output with 8 comparisons.


## Challenges

Challenges Faced
Understanding Merge Sort Algorithm: The recursive nature of merge sort was initially challenging to grasp, especially the divide-and-conquer approach and how the merging process works.

Tracking Comparisons and Swaps: Determining what constitutes a "comparison" and "swap" in merge sort was tricky since it doesn't swap elements in place like bubble sort. I counted each element assignment during merging as a swap.

Array Management: Handling multiple arrays (left, right, result) and managing indices during the merge process required careful attention to avoid index out of bounds errors.

Recursive Implementation: Ensuring the base case was correctly implemented and that recursive calls properly divided the problem into smaller subproblems.

Memory Efficiency: Traditional merge sort creates many temporary arrays, which could be optimized for better memory usage, but I focused on clarity for this implementation.

## This is my code!

```.java

import java.util.Scanner;

public class MergeSort {
    private static int comparisons = 0;
    private static int swaps = 0;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Read input
        int size = scanner.nextInt();
        int[] array = new int[size];

        for (int i = 0; i < size; i++) {
            array[i] = scanner.nextInt();
        }

        // Output unsorted array
        System.out.print("unsorted: ");
        printArray(array);

        // Perform merge sort
        int[] sortedArray = mergeSort(array);

        // Output sorted array
        System.out.print("sorted:   ");
        printArray(sortedArray);

        // Output comparisons and swaps
        System.out.println("comparisons: " + comparisons);

        scanner.close();
    }

    public static int[] mergeSort(int[] array) {
        comparisons = 0;
        swaps = 0;

        if (array.length <= 1) {
            return array;
        }

        return mergeSortRecursive(array, 0, array.length - 1);
    }

    private static int[] mergeSortRecursive(int[] array, int left, int right) {
        if (left == right) {
            return new int[]{array[left]};
        }

        int mid = left + (right - left) / 2;

        // Recursively sort left and right halves
        int[] leftArray = mergeSortRecursive(array, left, mid);
        int[] rightArray = mergeSortRecursive(array, mid + 1, right);

        // Merge the sorted halves
        return merge(leftArray, rightArray);
    }

    private static int[] merge(int[] leftArray, int[] rightArray) {
        int[] result = new int[leftArray.length + rightArray.length];
        int i = 0, j = 0, k = 0;

        while (i < leftArray.length && j < rightArray.length) {
            comparisons++; // Count comparison
            if (leftArray[i] <= rightArray[j]) {
                result[k++] = leftArray[i++];
            } else {
                result[k++] = rightArray[j++];
            }
            swaps++; // Count swap (each assignment is considered a swap)
        }

        // Copy remaining elements from left array
        while (i < leftArray.length) {
            result[k++] = leftArray[i++];
            swaps++; // Count swap
        }

        // Copy remaining elements from right array
        while (j < rightArray.length) {
            result[k++] = rightArray[j++];
            swaps++; // Count swap
        }

        return result;
    }

    private static void printArray(int[] array) {
        for (int i = 0; i < array.length; i++) {
            System.out.print(array[i]);
            if (i < array.length - 1) {
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

unsorted: 3 2 1 5 9 8
sorted:   1 2 3 5 8 9
comparisons: 8

Process finished with exit code 0

```


# VIDEO EXPLANATION!


https://app.screencastify.com/watch/UxiMsnO4sAoVGAHo94LE?checkOrg=30498c8e-8a1a-49cb-811f-fc81a0db6696




