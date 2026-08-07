# Lab No. 04 | Inheritance, Abstraction and Interface in C#

## Objective

To understand how Inheritance, Abstraction, and Interfaces help us build reusable, maintainable, and scalable applications in C#.

---

# Why Are We Doing This Lab?

### What?

In this lab, we will learn three important Object-Oriented Programming concepts:

- Inheritance
- Abstraction
- Interface

---

### Why?

Imagine building a College Management System.

Without OOP principles:

- Code gets repeated
- Maintenance becomes difficult
- Adding new features becomes harder

These concepts help us:

- Reuse existing code
- Hide unnecessary details
- Define common behavior for multiple classes

---

### When?

These concepts are used in almost every real-world application:

- Banking Systems
- E-Commerce Applications
- Hospital Management Systems
- College ERP Systems
- Mobile Applications

---

### How?

We will create:

```text
           Person
              │
      ┌───────┴───────┐
      │               │
      ▼               ▼
   Student         Faculty
```

to understand inheritance,

then use abstraction and interfaces to build cleaner designs.

---

# Concept Overview

## Key Concepts

- Inheritance
- Abstraction
- Interface
- Base Class
- Derived Class
- Abstract Class
- Method Overriding

---

# Quick Theory

## Inheritance

- Inheritance creates an IS-A relationship between classes.
- A child class gets properties and methods from a parent class.
- The child class can also add its own unique features.
- It helps reuse code and avoid repetition.
- Write common code once in the parent class and use it in child classes.

### Formula

```text
Person
   │
   ▼
Student
```

Example:

```text
Student IS-A Person
```

```text
            Person
               │
    ┌──────────┴──────────┐
    │                     │
 Student             Faculty
```

Both Student and Faculty inherit common properties from Person.

## Implementation of inheritance 

### Step 1: Create Base (Parent) Class 

```c#
// Parent class

public class Person
{
    public string Name;
    public int Age;

    public void DisplayPerson()
    {
        Console.WriteLine("Name : " + Name);
        Console.WriteLine("Age  : " + Age);
    }
}
```

### Explanation

Class Person contains common information shared by all people like Students and Faculty.

---

### Step 2: Create Derived (Child) Class

```c#
public class Student : Person
{
    public int RollNo;
}
```

### Explanation

Student automatically receives:

- Name
- Age
- DisplayPerson(), from Person.

- This is code reusability.

---

### Inheritance Flow

```text
Person Created
      ↓
Student Inherits Person
      ↓
Student Gets Name
      ↓
Student Gets Age
      ↓
Student Gets DisplayPerson()
```


# Types of Inheritance

`Inheritance` can be categorized into different types based on `how classes are related to each other`.



<img width="815" height="548" alt="image" src="https://github.com/user-attachments/assets/498d8fe0-e15c-4e85-bede-73da6af2a2e8" />




---

## 1. Single Inheritance

A `single child` class `inherits` from a `single parent` class.

### Diagram

```text
Person
   │
   ▼
Student
```

### Example

```c#
// Parent class
public class Person
{
    public string Name { get; set; }
}

// Child class
public class Student : Person
{
    public int RollNo { get; set; }
}
```

### Explanation

- `Student` inherits from `Person`.
- `Student` can access all public and protected members of `Person`.
- Promotes code reusability.

---

## 2. Multi-Level Inheritance

A `child` class `inherits` from  `another child` class.

### Diagram

```text
Person
   │
   ▼
Student
   │
   ▼
Monitor
```

### Example

```c#
// Parent
public class Person
{
    public string Name { get; set; }
}

// Child Class of Person
public class Student : Person
{
    public int RollNo { get; set; }
}

// Child Class of Student
public class Monitor : Student
{
    public string Section { get; set; }
}
```

### Explanation

- `Student` inherits from `Person`.
- `Monitor` inherits from `Student`.
- `Monitor` gets features from both `Student` and `Person`.

---

## 3. Hierarchical Inheritance

`Multiple child` classes `inherit` from the `same parent` class.

### Diagram

```text
           Person
              │
      ┌───────┴───────┐
      │               │
      ▼               ▼
   Student         Faculty
```

### Example

```c#
// Parent Class
public class Person
{
    public string Name { get; set; }
}

// Child Class of Person
public class Student : Person
{
    public int RollNo { get; set; }
}

// Child Class of Person
public class Faculty : Person
{
    public int EmployeeId { get; set; }
}
```

### Explanation

- Both `Student` and `Faculty` inherit from `Person`.
- Common functionality is defined once inside the parent class.
- Reduces code duplication.

---

## 4. Multiple Inheritance

Multiple inheritance means a `class inherits` from `more than one parent` class.

### Diagram

```text
Class A      Class B
    \           /
     \         /
      \       /
       Class C
```

### Example

```c#
// Parent Class
class A
{
}

// Parent Class
class B
{
}

// Child Class Of A, B
class C : A, B
{
}
```

### Explanation

- Multiple inheritance through classes is **not supported in C#**.
- This avoids confusion and errors when two parent classes have the same member name.
- Although C# does not allow a class to inherit from multiple classes.
- C# supports multiple inheritance using **interfaces**.
---

# 5. Hybrid Inheritance

Hybrid Inheritance is a `combination of two or more inheritance` types.

**For example**, a hierarchy may combine:

* Hierarchical Inheritance
* Multi-Level Inheritance

into a single structure.

---

## Diagram

```text
           Person
              │
      ┌───────┴───────┐
      │               │
      ▼               ▼
   Student         Faculty
      │
      ▼
   Monitor
```

---

## Explanation

In this example:

```text
Person → Student
Person → Faculty
```

represents **Hierarchical Inheritance**.

And:

```text
Person → Student → Monitor
```

represents **Multi-Level Inheritance**.

Since multiple inheritance structures are combined together, this is called **Hybrid Inheritance**.

---

## Why Hybrid Inheritance is Important

- Hybrid Inheritance is used for complex real-world relationships.
- It combines different types of inheritance.
- It helps classes use features from multiple inheritance structures.
- It improves code reusability.
- It keeps code organized and easy to manage.

---

## Hybrid Inheritance in C#

C# does **not support Hybrid Inheritance through classes** because it **does not support Multiple Inheritance between classes**.

Consider the following structure:

```text
       A
      / \
     B   C
      \ /
       D
```

This can create ambiguity when both `B` and `C` contain methods with the same name.

This problem is known as the **Diamond Problem**.

Because of this, C# prevents Multiple and Hybrid Inheritance through classes.

---

## How C# Achieves Hybrid Inheritance

- C# achieves Hybrid Inheritance using Interfaces.
- An `Interface` is a **set of rules that a class must follow**.
- A class can implement multiple interfaces.
- This helps combine different inheritance structures.
- We will learn its implementation in the Interface section.

---

# Quick Summary

| Type             | Formula                     | Description                      | C# Class Support      |
| ---------------- | --------------------------- | -------------------------------- | --------------------- |
| **Single**       | 1 Parent → 1 Child          | One parent, one child            | ✅ Yes                 |
| **Multi-Level**  | Parent → Child → Grandchild | Inheritance in levels            | ✅ Yes                 |
| **Hierarchical** | 1 Parent → Many Children    | One parent, many children        | ✅ Yes                 |
| **Multiple**     | Many Parents → 1 Child      | Multiple parents, one child      | ❌ No (Use Interfaces) |
| **Hybrid**       | Mix of Two or More Types    | Combination of inheritance types | ❌ No (Use Interfaces) |


> **Note:** C# supports Single, Multi-Level, and Hierarchical Inheritance through classes. 
>- Multiple and Hybrid Inheritance are achieved using Interfaces instead of classes.

---



## Interface

- An Interface is a set of rules.
- It tells a class what to do.
- It does not tell how to do it.
- A class that implements an interface must follow all rules.
- The class provides its own implementation.
- **Cannot create objects of interface directly** — An interface only defines contracts, not actual implementations.
- **Why?** It has no concrete logic/code inside its methods, only method signatures. An object needs actual behavior to exist.

**This promotes:**

- Loose Coupling
- Flexibility
- Consistency
- Scalability

**An Interface focuses on:**

-  **Interface** = Rules/Contract
-  **What** to do, **not how** to do it
-  Implementing class must provide the code.

## Implementation

## Create Interface

```c#
public interface Person
{
    void ShowRole();
}
```

---

## Implement Interface

```c#
// Child class
public class Student : Person
{
    public void ShowRole()
    {
        Console.WriteLine("I am a Student");
    }
}

// Child class
public class Faculty : Person
{
    public void ShowRole()
    {
        Console.WriteLine("I am a Faculty");
    }
}

```

---

## Explanation

An interface creates a contract (an agreement) between the interface and implementing classes.

It forces every class implementing it to provide actual implementations of all the interface methods.

In the example above, both `Student` and `Faculty` must implement the `ShowRole()` method.

---

## Interface Flow

```text
Person
   ↓
Defines ShowRole()
   ↓
Student & Faculty Implement Interface
   ↓
Provide Their Own ShowRole() Logic
```

# Program Output

```text
Name : Rahul
Age  : 20
Role : Student

Name : Amit
Age  : 35
Role : Faculty

I am a Student

I am a Faculty
```
---


## Implementation of multiple inheritance using interface
### Example

```c#
public interface Student
{
    void Study();
}

public interface Faculty
{
    void Teach();
}

public class Person : Student, Faculty
{
    public void Study()
    {
        Console.WriteLine("Student is Studying");
    }

    public void Teach()
    {
        Console.WriteLine("Faculty is Teaching");
    }
}
```

### Explanation

- A class can implement more than one interface.
- This allows a class to follow multiple sets of rules.
- It gives more flexibility.
- It avoids the problems of multiple class inheritance.
---

## Implementation of Hybrid Inheritance using Interface

### Example

When a single class needs to inherit behavior from multiple sources (like both Student and Faculty characteristics), we use multiple interfaces:
```c#
public interface Student
{
    void Study();
}

public interface Faculty
{
    void Teach();
}

public class Person : Student, Faculty
{
    public void Study()
    {
        Console.WriteLine("Student is Studying");
    }

    public void Teach()
    {
        Console.WriteLine("Faculty is Teaching");
    }
}
```

In this example:
- `Person` implements multiple interfaces.
- `Student` defines `Study()`.
- `Faculty` defines `Teach()`.
- `Person` provides implementations for both methods.
- This achieves Hybrid Inheritance behavior without the Diamond Problem.
- Ambiguity is avoided because interfaces don't contain conflicting implementations. 
---

# Class Hierarchy Diagram

```text
                      +----------------+
                      |    Person      |
                      +----------------+
                      | Study()        |
                      | Teach()        |
                      +----------------+
                              ▲
                              |
          +-------------------+------------------+
          |                                      |
          ▼                                      ▼

    +-------------+                     +-------------+
    |  Student   |                     |   Faculty  |
    +-------------+                     +-------------+
    | Study()     |                     | Teach()     |
    +-------------+                     +-------------+

```

## Abstraction
- Abstraction shows only the important features.
- It hides internal working details.
- Users focus on what an object does, not how it works.
- Details are not removed, only hidden from users.
- It makes programs simple and easy to use.
- **Cannot create objects of abstract class directly** — An abstract class contains one or more abstract methods with no implementation.
- **Why?** It's incomplete. You cannot instantiate an incomplete blueprint. You must create a derived class that provides actual implementation for all abstract methods.

<img width="624" height="345" alt="image" src="https://github.com/user-attachments/assets/cc43da6f-376c-42d7-8377-7aa29e28ee44" />



Example:

```text
ATM Machine

You know:
✔ Withdraw Money
✔ Check Balance

You do not know:
✖ Internal Banking Logic
✖ Database Queries
```

```text
Car

You know:
✔ Start()
✔ Stop()

You don't know:
✖ Engine Combustion Process
✖ Fuel Injection Logic
```

<img width="913" height="507" alt="image" src="https://github.com/user-attachments/assets/605934c4-609c-41d9-ac60-2630051d1c83" />


## implementation:

## Create Abstract Class

```c#
// Parent Class
public abstract class Person
{
    public abstract void ShowRole();
}
```

---

## Derived (Child) Class

```c#
// Child Class - Student
public class Student : Person
{
    public override void ShowRole()
    {
        Console.WriteLine("I am a Student");
    }
}

// Child Class - Faculty
public class Faculty : Person
{
    public override void ShowRole()
    {
        Console.WriteLine("I am a Faculty");
    }
}
```

---

## Explanation

Person says:

```text
Every Person MUST have a ShowRole() method.
```

But Person does not know what role to show.

Student and Faculty provide their own implementation.
- Student → "I am a Student"
- Faculty → "I am a Faculty"

---

## Abstraction Flow

```text
    Person
(Abstract Class)
       ↓
Declares Method
       ↓
Student, Faculty
       ↓
Provides Logic
```

---

# Full Concept Comparison

| Feature             | Inheritance| Abstraction         | Interface       |
|---------------------|------------|---------------------|-----------------|
| Purpose             | Reuse Code | Hide Implementation | Define Contract |
| Keyword             | :          | abstract            | interface       |
| Method Body Allowed | Yes        | Partial             | No              |
| Constructor Allowed | Yes        | Yes                 | No              |
| Multiple Support    | No         | No                  | Yes             |

---


## Non-Abstract Method in C#

A **non-abstract method** is a method that already contains its implementation (body). Since the logic is already written, any object of the class can directly use the method without needing to override it.

### Example

```csharp
using System;

abstract class Animal // an abstract class can contain both the method non abstract and abstract method lets have an example 
{
    // Non-abstract method
    public void Sleep()
    {
        Console.WriteLine("Every animal needs sleep.");
    }

    // Abstract method
    public abstract void MakeSound();
}

class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Dog barks.");
    }
}

class Program
{
    static void Main()
    {
        Dog dog = new Dog();

        dog.Sleep();       // Calls the inherited non-abstract method
        dog.MakeSound();   // Calls the overridden abstract method
    }
}
```

### Output

```text
Every animal needs sleep.
Dog barks.
```

---

## Explanation

### What is happening?

The `Animal` class contains:

- A **non-abstract method** named `Sleep()`.
- An **abstract method** named `MakeSound()`.

The `Dog` class inherits from `Animal`.

Since `Sleep()` already has an implementation, the `Dog` class automatically inherits it and can use it directly without writing the method again.

However, `MakeSound()` is abstract, so the `Dog` class **must** provide its own implementation using the `override` keyword.

---

## Why use Non-Abstract Methods?

Non-abstract methods are useful when the parent class already knows how a task should be performed and the same behavior can be shared by all child classes.

For example:

- Every animal sleeps.
- Every employee receives a salary.
- Every vehicle can start.
- Every student has a registration number.

Instead of writing the same code in every child class, we write it once in the parent class and allow all child classes to inherit it.

---

## Key Points

- A non-abstract method **contains a method body**.
- It can be called directly by objects of derived classes.
- Overriding is **optional**, not mandatory.
- It promotes **code reuse** by allowing common functionality to be inherited.

---

## Mental Model

```text
             Animal (Abstract Class)
             ┌─────────────────────┐
             │ Sleep()             │  ← Already implemented
             │ MakeSound()         │  ← Abstract
             └─────────┬───────────┘
                       │
                       ▼
                  Dog (Child Class)
             ┌─────────────────────┐
             │ Inherits Sleep()    │
             │ Implements          │
             │ MakeSound()         │
             └─────────────────────┘
```

### Rule to Remember

> **A non-abstract method provides a ready-made implementation that child classes inherit and use directly, whereas an abstract method only defines what should be done and requires child classes to implement how it is done.**


## Demo 1 (Inheritance)

Create a base class `Staff` and a derived class `Doctor` to calculate total salary based on a basic pay and specialized doctor allowances.
```
           using System;

           class Staff
           {
               protected string Name;
               protected double BasicPay;
           
               public Staff(string name, double basicPay)
               {
                   Name = name;
                   BasicPay = basicPay;
               }
           
               public virtual double CalculateSalary()
               {
                   return BasicPay;
               }
           
               public virtual void Display()
               {
                   Console.WriteLine("Name: " + Name);
                   Console.WriteLine("Basic Pay: " + BasicPay);
               }
           }
           
           class Doctor : Staff
           {
               private double DoctorAllowance;
           
               public Doctor(string name, double basicPay, double doctorAllowance)
                   : base(name, basicPay)
               {
                   DoctorAllowance = doctorAllowance;
               }
           
               public override double CalculateSalary()
               {
                   return BasicPay + DoctorAllowance;
               }
           
               public override void Display()
               {
                   base.Display();
                   Console.WriteLine("Doctor Allowance: " + DoctorAllowance);
                   Console.WriteLine("Total Salary: " + CalculateSalary());
               }
           }
           
           class Program
           {
               static void Main()
               {
                   Console.Write("Enter Doctor Name: ");
                   string name = Console.ReadLine();
           
                   Console.Write("Enter Basic Pay: ");
                   double basicPay = Convert.ToDouble(Console.ReadLine());
           
                   Console.Write("Enter Doctor Allowance: ");
                   double allowance = Convert.ToDouble(Console.ReadLine());
           
                   Doctor d = new Doctor(name, basicPay, allowance);
           
                   Console.WriteLine("\n--- Doctor Details ---");
                   d.Display();
               }
           }
```
### Explanation

**Staff (Base Class)**
- `Name` — Staff member's name
- `BasicPay` — Base salary
- `CalculateSalary()` — Method to calculate salary (marked as virtual for overriding)
- Constructor to initialize Name and BasicPay

**Doctor (Derived Class)**
- Inherits all properties and methods from Staff
- Adds `DoctorAllowance` — Additional allowance for doctors
- Overrides `CalculateSalary()` to include the doctor allowance in the calculation

**Formula** :
**Total Salary** = Basic Pay + Doctor Allowance

## Visualization :

```text
        Staff
          │
    ----------------
    Name
    BasicPay
    CalculateSalary()

          ▲
      Inherits

        Doctor
    ----------------
    DoctorAllowance
 CalculateSalary()
 ```
---

## Demo2(interface)
           using System;
           
           interface IPrinter
           {
               void Print();
           }
           
           class LaserPrinter : IPrinter
           {
               public void Print()
               {
                   Console.WriteLine("Printing using Laser Printer.");
               }
           }
           
           class InkjetPrinter : IPrinter
           {
               public void Print()
               {
                   Console.WriteLine("Printing using Inkjet Printer.");
               }
           }
           
           class Program
           {
               static void Main()
               {
                   IPrinter p1 = new LaserPrinter();
                   IPrinter p2 = new InkjetPrinter();
           
                   p1.Print();
                   p2.Print();
               }
           }

## Visualisation:

                     IPrinter
                    (Interface)
           
                     Print()
           
                     /      \
                    /        \
           
           LaserPrinter   InkjectPrinter
           
              Print()        Print()

---
## Demo3(Abstraction)
           using System;
           
           abstract class Vehicle
           {
               public string Brand;
           
               public Vehicle(string brand)
               {
                   Brand = brand;
               }
           
               // Abstract method (no implementation)
               public abstract void Start();
           
               // Normal method
               public void DisplayBrand()
               {
                   Console.WriteLine("Brand: " + Brand);
               }
           }
           
           class Car : Vehicle
           {
               public Car(string brand) : base(brand)
               {
               }
           
               public override void Start()
               {
                   Console.WriteLine("Car starts using key ignition.");
               }
           }
           
           class Program
           {
               static void Main()
               {
                   Car c = new Car("Toyota");
           
                   c.DisplayBrand();
                   c.Start();
               }
           }
## Visualization :
```text
              Vehicle
            (Abstract)
                |
                |
        + Start()      <-- No body
        + DisplayBrand()

              ↑

             Car
              |
        + Start()      <-- Implementation
```
---

# Common Errors

| Error                                | Cause                             | Solution                      |
|--------------------------------------|-----------------------------------|-------------------------------|
| Cannot instantiate abstract class    | Creating object of abstract class | Create derived class object   |
| Does not implement interface member  | Missing method implementation     | Implement all interface methods|
| Inheritance syntax error             | Missing ':'                       | Use ':' operator |
| No suitable method found to override | Method not marked correctly       | Use abstract/virtual keyword |

---

# One-Line Revision

```text
Inheritance → Reuse Code

Abstraction → Hide Complexity

Interface → Define Rules
```
# Lab Tasks :
1. Create a base class Staff and a derived class Doctor to calculate total salary 
based on a basic pay and specialized doctor allowances.

2. Create an abstract class Billing with an abstract method CalculateBill(). 
Implement this in InPatientBilling and OutPatientBilling classes.

3. Create an interface INotificationService and implement it across an 
EmailNotification and SMSNotification module. 

4. Create a base class MedicalEquipment and a derived class 
DiagnosticScanner to calculate maintenance costs based on a baseline 
service fee and specialized calibration allowances. 

5. Create an abstract class PatientRecord with an abstract method 
CompileReport(). Implement this in InpatientMedicalRecord and 
OutpatientMedicalRecord classes. 

6. Create an interface IInventoryManager and implement it across 
GroceryStock and ElectronicStock modules. Use exception handling to 
manage stock shortages and incorrect product details.
 ---
