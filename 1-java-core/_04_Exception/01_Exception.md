### Exception Handling in Java

1. **What is exception handling in Java?**  
Exception is an event that disrupts the normal flow of the program.   
Exception handling is a mechanism to handle runtime errors to maintain the normal flow of the application. It is used to handle the runtime errors that occur during the execution of a program.

2. **Advantages of Exception Handling in Java**

The core advantage of exception handling is maintaining the normal flow of the application. Consider the following advantages of exception handling in Java:
```
statement 1;  
statement 2;  
statement 3;  
statement 4;  
statement 5; // exception occurs
statement 6;  
statement 7;  
statement 8;  
statement 9;  
statement 10;  
```
If an exception occurs at statement 5, the rest of the code will not be executed. Exception handling helps to maintain the normal flow of the application.

3. **Types of Exceptions in Java**

There are two types of exceptions in Java:
- `Checked Exception`: These are the exceptions that are checked at compile time. If some code within a method throws a checked exception, then the method must either handle the exception or it must specify the exception using the `throws` keyword.
- `Unchecked Exception`: These are the exceptions that are not checked at compile time. It is up to the programmer to handle these exceptions. These are also called runtime exceptions.
- `Error`: These are not exceptions at all, but problems that arise beyond the control of the user or the programmer. Errors are typically ignored in your code because you can rarely do anything about an error. For example, if a stack overflow occurs, an error will arise. They are also unchecked exceptions.

5. **Exception Handling Keywords in Java**

Java provides five keywords for exception handling:
- `try`: The `try` block is used to enclose the code that might throw an exception.
- `catch`: The `catch` block is used to handle the exception. It must be preceded by a `try` block.
- `finally`: The `finally` block is used to execute important code such as closing a connection, whether an exception is thrown or not.
- `throw`: The `throw` keyword is used to throw an exception.
- `throws`: The `throws` keyword is used to declare an exception.