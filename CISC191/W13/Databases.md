# Manipulating databases using Java

### Armaan Shamsaasef
### Date: 12/7/2025
### CISC191 Week 13-14 Assignments


# Here is the flowchart!


<img width="658" height="954" alt="image" src="https://github.com/user-attachments/assets/c65acbb2-0975-4251-a62a-a236f57086a7" />


## Challenges

1. MySQL Installation & Configuration
MySQL PATH Issues: MySQL commands not recognized in PowerShell due to PATH not being set

PowerShell vs Command Prompt: Different behavior between PowerShell and CMD for running MySQL commands

Parentheses in Folder Names: mysql-connector-j-9.5.0 (4) folder name caused PowerShell parsing errors

Service Management: Starting/stopping MySQL service required administrative privileges

2. Database Setup
SQL Syntax Errors: Unclosed backticks () causing\c` errors in MySQL prompt

User Creation: Setting up java_user with correct permissions (CREATE USER, GRANT privileges)

Table Creation: Correct SQL syntax for CREATE TABLE with proper data types

3. Java Development Environment
Java Installation: JDK not initially installed or not in PATH (javac command not recognized)

Multiple Java Versions: JDK 25, JDK 21, and OpenJDK installations causing confusion

Environment Variables: Setting JAVA_HOME and adding to system PATH

Character Encoding: UTF-8 BOM (Byte Order Mark) causing compilation errors with \ufeff illegal character

4. MySQL Connector/J Issues
Nested Folder Structure: JAR file buried deep in mysql-connector-j-9.5.0 (4)\mysql-connector-j-9.5.0\mysql-connector-j-9.5.0.jar

JAR File Location: Finding and copying the correct JAR file to working directory

Classpath Configuration: Proper -cp syntax for compilation and execution

Version Compatibility: MySQL Server 8.0.44 with Connector/J 9.5.0 compatibility concerns

5. Code Implementation
PowerShell Code in Java File: Accidentally saving PowerShell script as Java file

SQL Injection Prevention: Switching from Statement to PreparedStatement for security

Date Formatting: Converting Java Date to SQL Date (1951-01-30 format)

Exception Handling: Proper try-catch blocks for SQL exceptions and connection failures

Resource Management: Closing Connection, Statement, and ResultSet objects properly

6. Database Connection in Java
JDBC URL Format: Correct syntax: jdbc:mysql://localhost:3306/Miramar

SSL/TLS Issues: Adding ?useSSL=false to connection string

Authentication: Switching between root and java_user credentials

Time Zone Issues: Adding &serverTimezone=UTC to avoid timezone warnings

Process & Workflow Challenges:
7. Development Workflow
Working Directory Management: Navigating between MySQL bin, Downloads, and Desktop folders

File Management: Creating, editing, and managing multiple files in different locations

Testing Iterations: Compile → Run → Debug → Fix cycle for each error

8. Tool Proficiency
PowerShell Commands: Learning cd, dir, Get-ChildItem, Copy-Item, etc.

MySQL Commands: CREATE DATABASE, CREATE TABLE, INSERT, UPDATE, SELECT

Java Commands: javac, java, classpath syntax with ; separator on Windows

9. Error Diagnosis & Resolution
Interpreting Error Messages: Understanding what each error meant and how to fix it

Sequential Problem Solving: Each fix revealed the next problem in the chain

Trial and Error: Multiple attempts to find the right syntax and configuration

Specific Error Messages You Encountered:
'mysql' is not recognized → PATH issue

A positional parameter cannot be found → Parentheses in folder names

ERROR 1064 (42000) → SQL syntax error with backticks

'javac' is not recognized → Java not installed/in PATH

illegal character: '\ufeff' → UTF-8 BOM encoding issue

Access denied for user → Incorrect database credentials

Communications link failure → MySQL service not running

No suitable driver found → Classpath or JDBC URL incorrect

Learning Curve Challenges:
10. Conceptual Understanding
JDBC Architecture: Understanding how Java connects to MySQL via driver

Database Transactions: INSERT, UPDATE, SELECT operations and their SQL syntax

Client-Server Model: MySQL as server, Java as client application

11. Integration Points
Java ↔ MySQL Communication: How the connector facilitates communication

Data Type Mapping: Java String ↔ SQL VARCHAR, Java Date ↔ SQL DATE

Connection Pooling: Single vs multiple database connections

Solutions Applied:
Used .\ prefix for commands in PowerShell

Quoted paths with parentheses: "mysql-connector-j-9.5.0 (4)"

Simplified workspace by moving to Desktop

Used ASCII encoding for Java files to avoid BOM

Full paths for Java commands when PATH wasn't set

Step-by-step debugging - fixing one error at a time

Verified each component separately (MySQL, Java, Connector) before integration

Key Lessons Learned:
Start Simple: Begin with basic connection before complex operations

Verify Each Step: Confirm MySQL works, then Java, then integration

Use Proper Tools: Notepad for Java, Command Prompt for MySQL (sometimes easier than PowerShell)

Document Everything: Keep track of paths, passwords, and configurations

Expect Encoding Issues: Always use ASCII/UTF-8 without BOM for Java files

## This is my code!

```.java
PS C:\Users\Armaan Shamsaasef\Desktop\JavaMySQL_Assignment> @'
>> import java.sql.*;
>>
>> public class MiramarStudentDB {
>>     public static void main(String[] args) {
>>         Connection conn = null;
>>
>>         try {
>>             // 1. Load MySQL Driver
>>             Class.forName("com.mysql.cj.jdbc.Driver");
>>             System.out.println("✓ MySQL Driver loaded!");
>>
>>             // 2. Connect to database
>>             conn = DriverManager.getConnection(
>>                 "jdbc:mysql://localhost:3306/Miramar?useSSL=false",
>>                 "java_user",
>>                 "JavaPass123!"
>>             );
>>             System.out.println("✓ Connected to Miramar database!");
>>
>>             // 3. Create statement
>>             Statement stmt = conn.createStatement();
>>
>>             // 4. Clear any existing record first
>>             try {
>>                 stmt.executeUpdate("DELETE FROM Student WHERE ssn = '111222333'");
>>                 System.out.println("Cleared any existing record");
>>             } catch (Exception e) {
>>                 // Table might not exist or be empty
>>             }
>>
>>             // 5. Insert student record
>>             System.out.println("\n=== INSERTING RECORD ===");
>>             String insertSQL = "INSERT INTO Student (ssn, firstName, mi, lastName, birthDate, street, phone, zipCode, deptId) " +
>>                               "VALUES ('111222333', 'Philip', 'David Charles', 'Collins', '1951-01-30', 'NA', 'NA', 'NA', '1234')";
>>             int rowsInserted = stmt.executeUpdate(insertSQL);
>>             System.out.println("✓ Inserted " + rowsInserted + " record");
>>
>>             // 6. Update zip code
>>             System.out.println("\n=== UPDATING ZIP CODE ===");
>>             String updateSQL = "UPDATE Student SET zipCode = '92126' WHERE ssn = '111222333'";
>>             int rowsUpdated = stmt.executeUpdate(updateSQL);
>>             System.out.println("✓ Updated " + rowsUpdated + " record");
>>
>>             // 7. Display results
>>             System.out.println("\n=== STUDENT RECORDS ===");
>>             ResultSet rs = stmt.executeQuery("SELECT * FROM Student");
>>
>>             System.out.println("SSN\t\tFirst Name\tLast Name\tZip Code\tDept ID");
>>             System.out.println("------------------------------------------------------------");
>>             while (rs.next()) {
>>                 System.out.println(
>>                     rs.getString("ssn") + "\t" +
>>                     rs.getString("firstName") + "\t\t" +
>>                     rs.getString("lastName") + "\t\t" +
>>                     rs.getString("zipCode") + "\t\t" +
>>                     rs.getString("deptId")
>>                 );
>>             }
>>             rs.close();
>>             stmt.close();
>>
>>             System.out.println("\n✓ All operations completed successfully!");
>>
>>         } catch (ClassNotFoundException e) {
>>             System.err.println("✗ MySQL Driver not found!");
>>             System.err.println("Make sure mysql-connector.jar is in classpath");
>>         } catch (SQLException e) {
>>             System.err.println("✗ Database error: " + e.getMessage());
>>             if (e.getMessage().contains("Access denied")) {
>>                 System.err.println("Check username/password: java_user/JavaPass123!");
>>             }
>>         } catch (Exception e) {
>>             e.printStackTrace();
>>         } finally {
>>             if (conn != null) {
>>                 try { conn.close(); } catch (SQLException e) {}
>>             }
>>         }
>>     }
>> }
>> '@ | Out-File -FilePath "MiramarStudentDB.java" -Encoding ASCII

```

## This is my result!



```.out

PS C:\Users\Armaan Shamsaasef\Desktop\JavaMySQL_Assignment> javac -cp "mysql-connector.jar" MiramarStudentDB.java
PS C:\Users\Armaan Shamsaasef\Desktop\JavaMySQL_Assignment>
PS C:\Users\Armaan Shamsaasef\Desktop\JavaMySQL_Assignment>
PS C:\Users\Armaan Shamsaasef\Desktop\JavaMySQL_Assignment> if ($LASTEXITCODE -eq 0) {
>>     Write-Host "✓ Compilation successful!" -ForegroundColor Green
>>     dir *.class
>> } else {
>>     Write-Host "✗ Compilation failed" -ForegroundColor Red
>> }
✓ Compilation successful!


    Directory: C:\Users\Armaan Shamsaasef\Desktop\JavaMySQL_Assignment


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         12/7/2025  10:07 AM           3864 MiramarStudentDB.class


PS C:\Users\Armaan Shamsaasef\Desktop\JavaMySQL_Assignment> java -cp "mysql-connector.jar;." MiramarStudentDB
? MySQL Driver loaded!
? Connected to Miramar database!
Cleared any existing record

=== INSERTING RECORD ===
? Inserted 1 record

=== UPDATING ZIP CODE ===
? Updated 1 record

=== STUDENT RECORDS ===
SSN             First Name      Last Name       Zip Code        Dept ID
------------------------------------------------------------
111222333       Philip          Collins         92126           1234

? All operations completed successfully!
PS C:\Users\Armaan Shamsaasef\Desktop\JavaMySQL_Assignment>

```


# VIDEO EXPLANATION!


https://app.screencastify.com/watch/mgPIASL8giVcwmxw4mJu?checkOrg=30498c8e-8a1a-49cb-811f-fc81a0db6696






