# Sorting

### Armaan Shamsaasef
### Date: 8/31/2025
### CISC Week 1 Assignments

# Flowchart

<img width="1484" height="4524" alt="image" src="https://github.com/user-attachments/assets/0903820e-6044-4c4c-88a8-120681ac40ca" />

# Explanation of my code.

##### I selected the bubble sort algorithm for this implementation for several reasons:

- Simplicity: Bubble sort is one of the simplest sorting algorithms to understand and implement, which makes it ideal for educational purposes and small datasets.

- Small Input Size: The problem states that the list will always contain less than 20 integers. For such small datasets, bubble sort's O(n²) time complexity is not a significant drawback, and its simplicity outweighs any performance concerns.

- Educational Value: Since this is a learning exercise, bubble sort provides a clear demonstration of how sorting works through simple comparison and swapping operations.

- In-place Sorting: Bubble sort works directly on the input array without requiring additional memory, which meets the requirement of modifying the array parameter.

# Challenges I faced.

- Array Index Management: Ensuring correct index boundaries in the nested loops of the bubble sort algorithm required careful attention to avoid off-by-one errors.

- Descending Order Implementation: Most sorting examples show ascending order, so I had to reverse the comparison logic to achieve descending order.

- Input Handling: Properly reading the input where the first integer indicates the count of following numbers required careful implementation to avoid reading errors.

- Output Formatting: The requirement to output the sorted array with commas between elements (but no comma at the end) needed special handling in the output loop.

- Algorithm Selection: While I chose bubble sort for its simplicity, I initially considered other algorithms but decided that the added complexity wouldn't provide benefits for the small input size specified.

# Here is my code!
```.java
import java.util.Scanner;

public class SortArray {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter the size of the array to be sorted: ");
        int n = scanner.nextInt();
        int[] arr = new int[n];

        System.out.print("Enter each number in the array and press enter.\n");
        for (int i = 0; i < n; i++) {
            arr[i] = scanner.nextInt();
        }

        sortArray(arr, n);

        System.out.print("Here is the sorted array: ");
        for (int i = 0; i < n; i++) {
            System.out.print(arr[i]);
            if (i < n - 1) {
                System.out.print(",");
            }
        }

        scanner.close();
    }

    public static void sortArray(int[] myArr, int arrSize) {
        for (int i = 0; i < arrSize - 1; i++) {
            for (int j = 0; j < arrSize - i - 1; j++) {
                if (myArr[j] < myArr[j + 1]) {
                    int temp = myArr[j];
                    myArr[j] = myArr[j + 1];
                    myArr[j + 1] = temp;
                }
            }
        }
    }
}

```

# Output

```.java

Enter the size of the array to be sorted: 5
Enter each number in the array and press enter.
12
54
56
15
4
Here is the sorted array: 56,54,15,12,4
Process finished with exit code 0

```

