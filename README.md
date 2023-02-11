# Exception Handling

## Learning Goals

- Explain exception handling.
- Use exception handling.

## Introduction

In Java, unexpected behavior is managed through exceptions. **Exceptions**
indicate an issue that occurs with the code upon execution. There are ways to
navigate these exceptions and that is through exception handling. **Exception**
**handling** is a way to handle exceptions through the process of "catching"
them. Exceptions can be "thrown" and "caught". The code that generates the
exception "throws" it and the code that called the method that generated the
exception can choose to "catch" it or "throw" it as well.

## Exception Handling in Java

To demonstrate exception handling, we can look back at an example from our user
interaction code:

```java
public static void main(String[] args) {
    System.out.println("Please enter a number:");
    Scanner inputScanner = new Scanner(System.in);
    int userNumber = inputScanner.nextInt();
    System.out.println("The user entered " + userNumber);
}
```

The `nextInt()` method of the `Scanner` class can throw an
`InputMismatchException`. It does so when the input entered by the user cannot
be interpreted as an integer number. When it throws an exception, it creates an
exception it wants to throw. For example, it might create an
`InputMismatchException` if the user did not enter an integer number. Once it
creates this exception, it will stop the process of wherever it may be in the
method, `nextInt()`, and return the exception to whoever originally called it.
Note that when an exception is thrown, the method will not return what it would
normally have returned if everything worked as expected.

The code that is receiving the exception can then choose to catch it by
wrapping the call for the method that throws it in a `try-catch` block, like
this:

```java
import java.util.InputMismatchException;
import java.util.Scanner;

public class StudentGame {
    public static void main(String[] args) {
      System.out.println("Please enter a number:");
      Scanner inputScanner = new Scanner(System.in);
      try {
        int userNumber = inputScanner.nextInt();
        System.out.println("The user entered " + userNumber);
      } catch (InputMismatchException exception) {
        System.out.println("The input was not a number");
      }
    }
}
```

A **try-catch block** tells the compiler that the code anticipates the
unexpected exceptions and that it is prepared to handle it. A `try` block can
have as many statements in it as we want and is enclosed with curly braces. Any
exceptions that are thrown by any code inside the `try` block will:

- Cause the execution of the statements inside the `try` block to be interrupted
  immediately.
- Cause the execution to jump straight past the remaining statements (if any)
  in the `try` block into the beginning of the `catch` block.
- The `catch` block takes in something called a parameter. More on parameters
  later when we learn about methods. For now, know that the `catch` block needs
  to be provided the type of exception that may be thrown. For example, if the
  code could possibly throw an `InputMismatchException`, like the code above,
  then we will pass that exception to the `catch` block.

Let's actually look at this flow of control by using the debugger! We'll set a
breakpoint on the line
`int userNumber = inputScanner.nextInt();`

![hit-breakpoint](https://curriculum-content.s3.amazonaws.com/java-mod-1/exception-handling/intellij-exception-handling-hit-breakpoint.PNG)

If we navigate over to the console and enter the word "Hello" instead of a
number and then choose the step-over action, we'll see that the execution jumps
into the `catch` block.

![catch-block](https://curriculum-content.s3.amazonaws.com/java-mod-1/exception-handling/intellij-catch-block.PNG)

Note that in the example above, the program will not execute the line
`System.out.println("The user entered " + userNumber);` if the user enters an
invalid input. This is because the exception was thrown on the previous line.
As we can see by running the program in the debugger, entering an invalid input
interrupts the execution of the `try` block. The execution will then jump right
to the `catch` block.

If we resume the program and look in the console, we will see that the output
is the message printed from the `catch` block:

```text
Please enter a number:
Hello
The input was not a number
```

The `InputMismatchException` is an "unchecked" exception, which means that the
code that could potentially cause it to be thrown does not _have_ to catch it.
This is in contrast to "checked" exceptions, which are exceptions that _must_ be
caught by any code that could potentially cause them to be thrown.

For example, Java provides methods for reading data from files. In most cases,
those methods throw a checked exception called `FileNotFoundException`, so that
the code that uses them _must_ catch that exception, or it will not compile.
Note that the `try {} catch {}` block for a checked exception follows exactly the
same structure as the `try {} catch {}` block for an unchecked exception.
