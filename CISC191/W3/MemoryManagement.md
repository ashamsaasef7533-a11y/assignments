# Memory Management

### Armaan Shamsaasef
### Date: 9/8/2025
### CISC191 Week 3 Assignments


# Here is the flowchart!

<img width="828" height="3882" alt="deepseek_mermaid_20250908_1bd840" src="https://github.com/user-attachments/assets/b7ec7fb8-826d-4d8c-bfe9-c17463f9cab5" />



## Explanation of my code.

Class Structure:

The StudentManagement class has two instance variables (className and studentCount) that are stored in the heap when objects are created

It contains methods to display information and modify student count

Object Creation:

In the main() method, we create a StudentManagement object which allocates memory in the heap

The reference variable classA is stored in the stack and points to the heap object

Method Execution:

When methods like displayInfo() and increaseStudents() are called, new stack frames are created

These frames contain method parameters and local variables

Garbage Collection Demonstration:

When we reassign classA to a new object, the original object becomes unreachable

The finalize() method (called by the garbage collector) demonstrates when objects are collected

System.gc() requests garbage collection (though it's not guaranteed to run immediately)

Memory Cleanup:

When the main() method ends, its stack frame is cleared

All remaining objects become eligible for garbage collection


## Challenges

Understanding Reference vs Object: Initially struggled with differentiating between the reference variable (on stack) and the actual object (on heap)

Garbage Collection Timing: Had to research how garbage collection works in Java since it's non-deterministic. Used System.gc() only for demonstration purposes.

Memory Visualization: Creating an accurate representation of stack and heap memory required careful consideration of what happens at each step of execution.

Finalize Method Limitations: Discovered that finalize() is deprecated since Java 9, but used it for demonstration as it clearly shows when an object is being collected.

Method Stack Frames: Understanding how each method call creates a new stack frame with local variables was challenging to visualize correctly.

## This is my code!

```.java

public class StudentManagement {
    private String className;
    private int studentCount;

    public StudentManagement(String className, int studentCount) {
        this.className = className;
        this.studentCount = studentCount;
    }

    public void displayInfo() {
        System.out.println("Class: " + className);
        System.out.println("Number of students: " + studentCount);
    }

    public void increaseStudents(int increment) {
        int previousCount = studentCount;
        studentCount += increment;
        System.out.println("Increased students from " + previousCount + " to " + studentCount);
    }

    public static void main(String[] args) {
        // Object creation - stored in heap
        StudentManagement classA = new StudentManagement("Computer Science", 30);

        // Method call - creates stack frame
        classA.displayInfo();

        // Another method call
        classA.increaseStudents(5);

        // Reassigning reference - makes previous object eligible for GC
        classA = new StudentManagement("Mathematics", 25);

        // Explicitly triggering GC (just for demonstration)
        System.gc();

        // Final method call
        classA.displayInfo();

        // Method ends, stack frame is cleared
    }

    // Finalize method to show when object is garbage collected
    @Override
    protected void finalize() throws Throwable {
        System.out.println("StudentManagement object for " + className + " is being garbage collected");
        super.finalize();
    }
}


/*

STACK MEMORY:
+-----------------------+
| main() method frame   |
|   args: [reference]   |
|   classA: [ref100] →  |
|                       |     HEAP MEMORY:
|                       |     +---------------------------+
|                       |     | Object at ref100           |
|                       |     | className: "Comp Science"  |
|                       |     | studentCount: 30           |
|                       |     | finalize(): [method ref]   |
|                       |     +---------------------------+
|                       |     
|                       |     After reassignment:
|                       |     +---------------------------+
|                       |     | Object at ref200           |
|                       |     | className: "Mathematics"   |
|                       |     | studentCount: 25           |
|                       |     | finalize(): [method ref]   |
|                       |     +---------------------------+
|                       |     
|                       |     Object at ref100 is now eligible
|                       |     for garbage collection
+-----------------------+
 */

```

## This is my result!

```.java

Class: Computer Science
Number of students: 30
Increased students from 30 to 35
Class: Mathematics
Number of students: 25

```


# VIDEO EXPLANATION!








