# Lab No. 05 | Exception Handling in C# 

# Learning Objectives

After completing this lab, you will be able to:

- Understand runtime errors and exceptions.
- Use `try`, `catch`, `finally`, and `throw`.
- Handle common exceptions in C#.
- Validate user input.
- Create custom exceptions.
- Apply exception handling in real-world applications.
- Use exception handling with Classes, Inheritance, and Interfaces.

---

# Why Are We Doing This Lab?

## What?

Exception Handling is a mechanism used to detect and manage runtime errors in a program.

Instead of crashing the application, exception handling allows the program to recover gracefully and display meaningful error messages.

---

## Why?

Without exception handling:

```text
Program Starts
      ↓
Runtime Error Occurs
      ↓
Program Crashes
```

With exception handling:

```text
Program Starts
      ↓
Runtime Error Occurs
      ↓
Exception Caught
      ↓
User-Friendly Message Displayed
      ↓
Program Continues Safely
```

---

## When?

Exception handling is used when:

- User enters invalid input
- Division by zero occurs
- Invalid file operations occur
- Database operations fail
- Network communication fails
- Business rules are violated

---

## How?

C# provides four primary keywords:

| Keyword | Purpose |
|----------|----------|
| try | Contains risky code |
| catch | Handles exceptions |
| finally | Executes regardless of exception |
| throw | Generates exceptions manually |

---

# Concept Overview

## Key Concepts

- Exception
- Exception Handling
- try
- catch
- finally
- throw
- Custom Exception

---

# Quick Theory

## What is an Exception?

An exception is an unexpected event that occurs while the program is executing.

Without exception handling, a program may terminate unexpectedly when an error occurs.

`Exception` is the parent class of all exception types in C#.

## Common Exception Examples

| Example | Exception Generated |
|----------|-------------------|
| `10 / 0` | `DivideByZeroException` |
| `Convert.ToInt32("ABC")` | `FormatException` |
| `int[] arr = new int[3]; arr[5]` | `IndexOutOfRangeException` |
| `string name = null; name.Length` | `NullReferenceException` |
| `checked { int n = int.MaxValue; n++; }` | `OverflowException` |
| `File.ReadAllText("Data.txt")` | `FileNotFoundException` |
| `(int)(object)"Hello"` | `InvalidCastException` |

---

## Real-Life Example

| Situation | Exception Scenario |
|------------|-------------------|
| ATM Withdrawal | Insufficient Balance |
| Student Marks Entry | Marks greater than 100 |
| Online Payment | Invalid Payment Amount |
| Login System | Invalid User Input |
| Speed Monitoring System | Speed exceeds limit |

---

# Types of Errors

## 1. Compile-Time Errors

Detected by the compiler before execution.

| Code | Error Message |
|--------|---------------|
| `int number = "Hello";` | `CS0029: Cannot implicitly convert type 'string' to 'int'` |
| `Console.WriteLine(x);` | `CS0103: The name 'x' does not exist in the current context` |
| `int a = 10 Console.WriteLine(a);` | `CS1002: ; expected` |

Example:

```csharp
int number = "Hello";
```

Error:

```text
Cannot implicitly convert type 'string' to 'int'
```

---

## 2. Runtime Errors (Exceptions)

Detected while the program executes.

| Code | Error Message |
|--------|---------------|
| `int result = 10 / 0;` | `System.DivideByZeroException: Attempted to divide by zero.` |
| `string name = null; Console.WriteLine(name.Length);` | `System.NullReferenceException: Object reference not set to an instance of an object.` |
| `int[] arr = {1,2,3}; Console.WriteLine(arr[5]);` | `System.IndexOutOfRangeException: Index was outside the bounds of the array.` |


Example:

```csharp
int result = 10 / 0;
```

Error:

```text
Unhandled exception.
System.DivideByZeroException:
Attempted to divide by zero.
```

---

## 3. Logical Errors

Program executes successfully but gives incorrect output.

| Code | Actual Output | Expected Output |
|--------|---------------|----------------|
| `int area = length + width;` | `Area = 15` | `Area = 50` |
| `if(age > 18)` | Age 18 is considered ineligible | Age 18 should be eligible |
| `average = total / 2;` (for 3 subjects) | Incorrect average | Correct average should use `/ 3` |


Example:

```csharp
int area = length + width;
```

instead of:

```csharp
int area = length * width;
```

---

# Exception Handling Flow

```text
                Start
                  │
                  ▼
            Execute try
                  │
         ┌────────┴────────┐
         │                 │
         ▼                 ▼
 No Exception      Exception Occurs
         │                 │
         ▼                 ▼
 Continue          Execute catch
         │                 │
         └────────┬────────┘
                  ▼
          Execute finally
                  │
                  ▼
                 End
```

---

# Basic Syntax of Exception Handling

```csharp
try
{
    // Risky code
}
catch(Exception ex)
{
    // Handle exception
}
finally
{
    // Cleanup code
}
```

---

# Example: Division by Zero Handling

```csharp
using System;

namespace ExceptionHandlingLab
{
    class Program
    {
        static void Main()
        {
            try
            {
                Console.Write("Enter Numerator: ");
                int numerator =
                    Convert.ToInt32(Console.ReadLine());

                Console.Write("Enter Denominator: ");
                int denominator =
                    Convert.ToInt32(Console.ReadLine());

                int result = numerator / denominator;

                Console.WriteLine(
                    $"Result = {result}");
            }
            catch (DivideByZeroException)
            {
                Console.WriteLine(
                    "Error: Denominator cannot be zero.");
            }
            catch (FormatException)
            {
                Console.WriteLine(
                    "Error: Invalid numeric input.");
            }
            finally
            {
                Console.WriteLine(
                    "Program execution completed.");
            }
        }
    }
}
```
--- 


# Custom Exception

### What is a Custom Exception?

A custom exception is a user-defined exception created specifically for application requirements.

Example:

```text
InsufficientBalanceException
```

is much more meaningful than:

```text
Exception
```

---

## Class Diagram

```text
                    Exception
                         ▲
                         │
                         │
     InsufficientBalanceException


                   BankAccount
         ┌─────────────────────────┐
         │ AccountNumber           │
         │ Balance                 │
         ├─────────────────────────┤
         │ Deposit()              │
         │ Withdraw()             │
         └─────────────────────────┘
```

---

## Example

```csharp
using System;

class InvalidAgeException : Exception
{
    public InvalidAgeException(string message)
        : base(message)
    {
    }
}

class Program
{
    static void Main()
    {
        try
        {
            int age = -5;

            if (age < 0)
            {
                throw new InvalidAgeException(
                    "Age cannot be negative.");
            }

            Console.WriteLine($"Age: {age}");
        }
        catch (InvalidAgeException ex)
        {
            Console.WriteLine(
                $"Error: {ex.Message}");
        }
    }
}
```

### Output

```text
Error: Age cannot be negative.
```
---

# Common Mistakes in Exception Handling

| Error                            | Cause                          | Solution                                |
|----------------------------------|--------------------------------|-----------------------------------------|
| Program crashes unexpectedly     | Exception is not handled       | Use `try-catch` blocks                  |
| Error message is not shown       | Empty `catch` block is used    | Display meaningful messages in `catch`  |
| Invalid input causes exception   | User input is not validated    | Validate input before conversion        |
| Custom exception is never raised | Missing `throw` statement      | Use `throw` when validation fails       |
| Invalid data is accepted         | Incorrect validation logic     | Verify conditions carefully             |
| Program fails for special cases  | Edge cases are not handled     | Check negative, zero, and empty values  |
| Wrong exception is caught        | Using only generic `Exception` | Catch specific exceptions when possible |
| Resources are not cleaned up     | Missing `finally` block        | Use `finally` for cleanup operations    |

---

# Student Tasks

## Section A
1. Write a program to perform division of two numbers entered by the user. Use exception handling to catch and display an appropriate message when the denominator is zero.

2. Write a program to accept a numeric value from the user. Use exception handling to detect and handle invalid input when non-numeric data is entered. 

3. Write a program to create a BankAccount class with account number and balance. Use exception handling to display an error message when the withdrawal amount exceeds the available balance. 

4. Write a program to create a Student class with student details and marks. Use exception handling to generate an error when marks entered are less than 0 or greater than 100. 

## Section B
5. Write a program to create a base class Employee and a derived class Manager. Use a custom exception to handle cases where the salary entered is negative.

6. Write a program to create a base class Vehicle and a derived class Car. Use exception handling to display an error message when the speed entered exceeds the permitted limit. 

7. Write a program to create an interface ITransaction with methods for deposit and withdrawal. Implement the interface in a class and use exception handling to manage invalid withdrawal transactions.

8. Write a program to create an interface IPayment with a method to process payments. Implement the interface in different payment classes and use exception handling to handle invalid payment amounts or transaction failures.

---

# Additional Practice Programs

1. Age Validation System
2. Voting Eligibility Checker
3. Student Grade Calculator
4. Number Guessing Game
5. ATM Withdrawal Validation
