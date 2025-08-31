# WordFrequencies

### Armaan Shamsaasef
### Date: 8/31/2025
### CISC191 Week 1 Assignments


# Here is the flowchart!


<img width="2036" height="4800" alt="image" src="https://github.com/user-attachments/assets/2400d31a-e807-4ea2-9c8c-94512ed169e7" />


## Explanation of my code.

This sorting algorithm consists of an array that loops into the input string converted into lowercase and then counts the words.

## Challenge

Drawing the flowchart is what was the most challenging to me because I have never done it before and it took a long time to have my ideas flow. However, I take this as a learning experience.


## This is my code!

```.java

import java.util.Scanner;
import java.util.HashMap;

public class WordFrequencies {

    public static int getWordFrequency(String[] wordsList, int listSize, String currWord) {
        int count = 0;
        String searchWordLower = currWord.toLowerCase();

        for (int i = 0; i < listSize; i++) {
            if (wordsList[i].toLowerCase().equals(searchWordLower)) {
                count++;
            }
        }

        return count;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Enter words separated by spaces:");
        String input = sc.nextLine();

        String[] words = input.split("\\s+");
        int size = words.length;

        // Use a HashMap for efficient frequency counting
        HashMap<String, Integer> frequencyMap = new HashMap<>();

        for (String word : words) {
            String wordLower = word.toLowerCase();
            frequencyMap.put(wordLower, frequencyMap.getOrDefault(wordLower, 0) + 1);
        }

        // Output words with their frequencies
        for (String word : words) {
            // Using the frequency map for efficiency rather than calling getWordFrequency repeatedly
            int frequency = frequencyMap.get(word.toLowerCase());
            System.out.println(word + " " + frequency);
        }

        sc.close();
    }
}
```

## This is my result!

```.out
Enter words separated by spaces:
The quick brown fox jumps over the lazy dog
The 2
quick 1
brown 1
fox 1
jumps 1
over 1
the 2
lazy 1
dog 1

Process finished with exit code 0



```






