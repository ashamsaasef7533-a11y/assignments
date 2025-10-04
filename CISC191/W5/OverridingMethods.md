### Armaan Shamsaasef
### Date: 10/3/2025
### CISC191 Week 5 Assignments


# Here is the flowchart!


<img width="521" height="922" alt="image" src="https://github.com/user-attachments/assets/a2d86edd-5730-411d-aaf1-ec6f6a1a43bc" />



## Explanation of my code.

The code demonstrates method overriding in Java inheritance:

Book Class: Base class with basic book properties and a printInfo() method

Encyclopedia Class: Derived class that:

Adds two new fields (edition and numberOfPages)

Overrides the printInfo() method to include the additional fields

Uses getter methods to access private fields from the parent class

Main Class: Handles input reading and object creation

The key concept shown is method overriding - the Encyclopedia class provides its own implementation of printInfo() that extends the functionality of the base class method while maintaining the same method signature.

## Challenges

Challenges Faced
Input Handling: Reading multiple lines of input in the correct order for both Book and Encyclopedia objects

Method Overriding: Ensuring the overridden printInfo() method in Encyclopedia properly calls the base class method while adding new functionality

Access Control: Managing private fields with appropriate getters/setters while maintaining encapsulation

Input Order: Keeping track of which input corresponds to which field, especially when reading both Book and Encyclopedia data sequentially




## This is my code!


#### Book.java

```.java

// Base Book class
public class Book {
    // Private fields
    private String title;
    private String author;
    private String publisher;
    private String publicationDate;

    // Default constructor
    public Book() {}

    // Getter methods
    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public String getPublisher() {
        return publisher;
    }

    public String getPublicationDate() {
        return publicationDate;
    }

    // Setter methods
    public void setTitle(String title) {
        this.title = title;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public void setPublisher(String publisher) {
        this.publisher = publisher;
    }

    public void setPublicationDate(String publicationDate) {
        this.publicationDate = publicationDate;
    }

    // Method to be overridden
    public void printInfo() {
        System.out.println("Book Information: ");
        System.out.println("   Book Title: " + title);
        System.out.println("   Author: " + author);
        System.out.println("   Publisher: " + publisher);
        System.out.println("   Publication Date: " + publicationDate);
    }
}

```

#### Encyclopedia.java



```.java

// Derived Encyclopedia class
public class Encyclopedia extends Book {
    // Additional private fields
    private String edition;
    private int numberOfPages;

    // Default constructor
    public Encyclopedia() {}

    // Getter methods for new fields
    public String getEdition() {
        return edition;
    }

    public int getNumberOfPages() {
        return numberOfPages;
    }

    // Setter methods for new fields
    public void setEdition(String edition) {
        this.edition = edition;
    }

    public void setNumberOfPages(int numberOfPages) {
        this.numberOfPages = numberOfPages;
    }

    // Overriding the printInfo method
    @Override
    public void printInfo() {
        System.out.println("Book Information: ");
        System.out.println("   Book Title: " + getTitle());
        System.out.println("   Author: " + getAuthor());
        System.out.println("   Publisher: " + getPublisher());
        System.out.println("   Publication Date: " + getPublicationDate());
        System.out.println("   Edition: " + edition);
        System.out.println("   Number of Pages: " + numberOfPages);
    }
}

```



#### Main.java

```java.

import java.util.Scanner;

// Main class
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Create a Book object
        Book book = new Book();

        // Read input for Book
        book.setTitle(scanner.nextLine());
        book.setAuthor(scanner.nextLine());
        book.setPublisher(scanner.nextLine());
        book.setPublicationDate(scanner.nextLine());

        // Create an Encyclopedia object
        Encyclopedia encyclopedia = new Encyclopedia();

        // Read input for Encyclopedia
        encyclopedia.setTitle(scanner.nextLine());
        encyclopedia.setAuthor(scanner.nextLine());
        encyclopedia.setPublisher(scanner.nextLine());
        encyclopedia.setPublicationDate(scanner.nextLine());
        encyclopedia.setEdition(scanner.nextLine());
        encyclopedia.setNumberOfPages(scanner.nextInt());

        // Close scanner
        scanner.close();

        // Print information
        book.printInfo();
        encyclopedia.printInfo();
    }
}

```

## This is my result!

```.out

The Hobbit
J. R. R. Tolkien
George Allen & Unwin
21 September 1937
The Illustrated Encyclopedia of the Universe
Ian Ridpath
Watson-Guptill
2001
2nd
384
Book Information: 
   Book Title: The Hobbit
   Author: J. R. R. Tolkien
   Publisher: George Allen & Unwin
   Publication Date: 21 September 1937
Book Information: 
   Book Title: The Illustrated Encyclopedia of the Universe
   Author: Ian Ridpath
   Publisher: Watson-Guptill
   Publication Date: 2001
   Edition: 2nd
   Number of Pages: 384

Process finished with exit code 0

```


# VIDEO EXPLANATION!


https://app.screencastify.com/watch/Jr3nv62dMBYodhXhGYuS


