
### Armaan Shamsaasef
### Date: 9/20/2025
### CISC191 Week 4 Assignments


# Here is the flowchart!

<img width="232" height="771" alt="image" src="https://github.com/user-attachments/assets/d1a8150a-8bf1-4e40-a7ba-d3bbc806a236" />



## Explanation of my code.

This Java program reads a text file containing filenames and converts photo filenames to info filenames:

Input Handling: Uses Scanner to get the input filename from the user

File Reading: Uses BufferedReader to read the file line by line

Text Processing: Uses regex to replace "_photo.jpg" with "_info.txt" at the end of each line

Output: Prints each modified line to the console

Error Handling: Catches and displays IOExceptions

## Challenges

File Path Issues: Forgetting to include the full path when the file isn't in the current directory

Using incorrect path separators (Windows vs. Unix)

File Permissions: Trying to read a file without proper read permissions

File Not Found: Entering an incorrect filename or path

File extension confusion (.txt vs other extensions)

Regex Pattern Issues: The pattern might not match if filenames have variations

Encoding Issues: The file might have a different character encoding than expected

## This is my code!

```.java

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.Scanner;

public class PhotoFileConverter {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Read the input file name
        System.out.print("Enter the name of the text file (with full path): ");
        String inputFileName = scanner.nextLine();

        try (BufferedReader reader = new BufferedReader(new FileReader(inputFileName))) {
            String line;

            // Read each line from the file
            while ((line = reader.readLine()) != null) {
                // Use regex to replace "_photo.jpg" with "_info.txt"
                String modifiedLine = line.replaceAll("_photo\\.jpg$", "_info.txt");
                System.out.println(modifiedLine);
            }
        } catch (IOException e) {
            System.out.println("Error reading the file: " + e.getMessage());
        }

        scanner.close();
    }
}

```

## This is my result!

```.out

Enter the name of the text file (with full path): C:\Users\armaa\IdeaProjects\CISC191-W4\src/ParkPhotos.txt
Acadia2003_info.txt
AmericanSamoa1989_info.txt
BlackCanyonoftheGunnison1983_info.txt
CarlsbadCaverns2010_info.txt
CraterLake1996_info.txt
GrandCanyon1996_info.txt
IndianaDunes1987_info.txt
LakeClark2009_info.txt
Redwood1980_info.txt
VirginIslands2007_info.txt
Voyageurs2006_info.txt
WrangellStElias1987_info.txt

Process finished with exit code 0

```


# VIDEO EXPLANATION!


https://app.screencastify.com/watch/qCexePPlYJKB6jn8etc5





