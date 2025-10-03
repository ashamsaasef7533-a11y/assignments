### Armaan Shamsaasef
### Date: 10/3/2025
### CISC191 Week 5 Assignments


# Here is the flowchart!

<img width="398" height="903" alt="image" src="https://github.com/user-attachments/assets/92593318-8093-454e-8d51-1debef643356" />




## Explanation of my code.

The solution consists of three main components:

Course (Base Class):

Contains private fields for course number and title

Provides public getter and setter methods for encapsulation

Implements printInfo() method to display basic course information

OfferedCourse (Derived Class):

Extends the Course class to inherit its properties and methods

Adds three new private fields specific to offered courses

Overrides the printInfo() method to include additional information while reusing base class functionality

CourseManagement (Main Class):

Creates instances of both base and derived classes

Demonstrates inheritance by showing how the derived class can access base class methods

Produces the exact output format as specified in the requirements

The program demonstrates key OOP principles: encapsulation, inheritance, and method overriding.

## Challenges

Challenges Faced
Design Phase Challenges:

Determining the appropriate level of encapsulation for class fields

Deciding which methods to override vs. which to inherit directly

Planning the class hierarchy to maximize code reuse

Implementation Phase Challenges:

Ensuring proper access to private fields through getter/setter methods

Correctly overriding the printInfo() method while maintaining the base functionality

Managing the constructor chain in inheritance (though we used setters in this case)

Maintaining the exact output format as specified in the requirements

Testing Challenges:

Verifying that both base and derived classes work independently

Ensuring the output matches the expected format exactly

Testing the inheritance relationship works correctly



## This is my code!


#### Main.java

```.java
// Base class
class Course {
    private String courseNumber;
    private String courseTitle;

    // Setter methods
    public void setCourseNumber(String courseNumber) {
        this.courseNumber = courseNumber;
    }

    public void setCourseTitle(String courseTitle) {
        this.courseTitle = courseTitle;
    }

    // Getter methods
    public String getCourseNumber() {
        return courseNumber;
    }

    public String getCourseTitle() {
        return courseTitle;
    }

    // PrintInfo method
    public void printInfo() {
        System.out.println("Course Information:");
        System.out.println("   Course Number: " + courseNumber);
        System.out.println("   Course Title: " + courseTitle);
    }
}

// Derived class
class OfferedCourse extends Course {
    private String instructorName;
    private String location;
    private String classTime;

    // Setter methods
    public void setInstructorName(String instructorName) {
        this.instructorName = instructorName;
    }

    public void setLocation(String location) {
        this.location = location;
    }

    public void setClassTime(String classTime) {
        this.classTime = classTime;
    }

    // Getter methods
    public String getInstructorName() {
        return instructorName;
    }

    public String getLocation() {
        return location;
    }

    public String getClassTime() {
        return classTime;
    }

    // Override PrintInfo method
    @Override
    public void printInfo() {
        System.out.println("Course Information:");
        System.out.println("   Course Number: " + getCourseNumber());
        System.out.println("   Course Title: " + getCourseTitle());
        System.out.println("   Instructor Name: " + instructorName);
        System.out.println("   Location: " + location);
        System.out.println("   Class Time: " + classTime);
    }
}
```

#### CourseManagement.java



```.java

// Main class
public class CourseManagement {
    public static void main(String[] args) {
        // Create first course (base class)
        Course course1 = new Course();
        course1.setCourseNumber("ECE287");
        course1.setCourseTitle("Digital Systems Design");
        course1.printInfo();

        System.out.println();

        // Create second course (derived class)
        OfferedCourse course2 = new OfferedCourse();
        course2.setCourseNumber("ECE387");
        course2.setCourseTitle("Embedded Systems Design");
        course2.setInstructorName("Mark Patterson");
        course2.setLocation("Wilson Hall 231");
        course2.setClassTime("WF: 2-3:30 pm");
        course2.printInfo();
    }
}

```

## This is my result!

```.out

Course Information:
   Course Number: ECE287
   Course Title: Digital Systems Design

Course Information:
   Course Number: ECE387
   Course Title: Embedded Systems Design
   Instructor Name: Mark Patterson
   Location: Wilson Hall 231
   Class Time: WF: 2-3:30 pm

Process finished with exit code 0

```


# VIDEO EXPLANATION!



