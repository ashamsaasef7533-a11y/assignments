### Armaan Shamsaasef
### Date: 9/23/2025
### CISC191 Week 5 Assignments


# Here is the flowchart!

<img width="154" height="727" alt="image" src="https://github.com/user-attachments/assets/e0447e86-94ce-46df-a3bc-f876a3d0f21e" />



## Explanation of my code.

Inheritance: Student is a subclass of Person, so it gets all of Person's methods

Method Overriding: Student could override printAll() but doesn't in this case

Object Creation: new Student() creates an object with both parent and child attributes

Method Chaining: Sequential calls to set up the object's state before printing

## Why This Works:


Student "is-a" Person (inheritance relationship)

Private fields are accessible only through public methods

printAll() + manual ID printing solves the incomplete output problem

## Challenges

The main challenges I faced involved understanding Java inheritance where Student extends Person, requiring recognition that Student objects inherit Person's methods like setName() and setAge(). As well as: dealing with the incomplete printAll() method that didn't include ID information, necessitating creative output extension, properly sequencing method calls with correct syntax, achieving exact output formatting by combining inherited and manual printing, and grasping object creation concepts where a Student instance can access both parent and child class methods through polymorphism.




## This is my code!


#### Person.java

```.java
// ===== Code from file Person.java =====
public class Person {
    private int ageYears;
    private String lastName;

    public void setName(String userName) {
        lastName  = userName;
    }

    public void setAge(int numYears) {
        ageYears = numYears;
    }

    // Other parts omitted

    public void printAll() {
        System.out.print("Name: " + lastName);
        System.out.print(", Age: "  + ageYears);
    }
}
// ===== end =====
```

#### Student.java
```.java

// ===== Code from file Student.java =====
public class Student extends Person {
    private int idNum;

    public void setID(int studentId) {
        idNum = studentId;
    }

    public int getID() {
        return idNum;
    }
}
// ===== end =====
```


#### StudentDerivationFromPerson.java
```.java
public class StudentDerivationFromPerson {
    public static void main(String[] args) {
        Student courseStudent = new Student();

        /* Your solution goes here  */
        courseStudent.setName("Smith");
        courseStudent.setAge(20);
        courseStudent.setID(9999);

        courseStudent.printAll();
        System.out.print(", ID: " + courseStudent.getID());
    }
}
```

## This is my result!

```.out

Name: Smith, Age: 20, ID: 9999

Process finished with exit code 0

```


# VIDEO EXPLANATION!



