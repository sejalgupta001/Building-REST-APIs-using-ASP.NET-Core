# Lab No. 03 | Class, Objects, Array of Objects & Constructor in C#

## Objective

To understand and implement classes, objects, constructors, member functions, and arrays of objects in C# through console applications.

---
# Why Are We Doing This Lab?

## What?

In this lab, we learn how to store related data and functions together using C# programs.

For example, one faculty member has ID, name, age, weight, and height. Instead of keeping these values separately everywhere, we keep them together in one proper format.

## Why?

Object-oriented programming helps us write clean and reusable programs. It is useful when a program needs to work with real-world things like faculty, employees, cars, items, and patients.

## When?

We use this idea when a program needs to store details and actions together.

Examples:

- A college stores faculty details.
- A company stores employee details.
- A supermarket stores item details.
- A clinic stores patient details.

## How?

We create a common format first, then create records from that format and use functions to take input or display output.

---


# Class and Object Relationship Diagram
 
![Class and Object Relationship Diagram](https://res.cloudinary.com/dpdxoxtog/image/upload/v1781163443/ChatGPT_Image_Jun_11_2026_12_59_54_PM_aj7ejh.png)

## Explanation of Diagram

- `Faculty` is the common format.
- `faculty1`, `faculty2`, and `faculty3` are separate records.
- Each record can store different values.
- All records follow the same structure.

---

# Simple Real-Time Understanding of Class and Object

Think about a college ID card system.

The college decides that every faculty record should contain ID, name, age, weight, and height. This common format is like a ready plan for storing faculty information.

Now, when we enter details for one real faculty member, such as:

```text
ID: 101
Name: Raj Mehta
Age: 35
Weight: 72
Height: 175
```

that one filled record becomes one usable faculty record in the program.

In the same way, if the college enters another faculty member's details, that becomes another separate record. Both records follow the same format, but their values are different.

So in programming, we first prepare the common format, and then we create separate records from it whenever needed.

---

# Concept Overview

## Key Concepts

- Class
- Object
- Data members
- Member functions
- Array of objects
- Constructor
- Console input and output

---


# Important Theory

## Class

A class is used to group related data and functions in one place.

A Class is a blueprint or template used to create objects. It defines the properties (data members) and behaviors (methods) that objects will have.

A class itself does not allocate memory for its non-static data members. Memory for those members is allocated only when an object (instance) of the class is created.

Real-time example:

In a college system, `Faculty` can be used to keep faculty-related details and actions together.

```csharp
class Faculty
{
    int id;
    string name;

    void DisplayFacultyDetails()
    {
        // Code to display faculty details
    }
}
```

Here, `id` and `name` store data, and `DisplayFacultyDetails()` performs an action.

## Object

An object is a real usable record created from a class.

An Object is an instance of a class. It occupies memory and can access the class's data members and methods.

# Why Use an Object?
To access class members.
To store actual values.
To perform operations defined in the class.
Real-time example:

If `Faculty` is the common format, then one faculty member like `Raj Mehta` becomes one object.

```csharp
Faculty faculty = new Faculty();
```

Here, `faculty` can store and display details for one faculty member.

## Data Members

Data members are variables declared inside a class. They store information about the object.

Example:

```csharp
int id;
string name;
int age;
```

In the faculty program, these variables store faculty ID, name, and age.

## Member Functions

Member functions are methods written inside a class. They perform operations using the class data.

A Method is a block of code that performs a specific task. It is defined inside a class and is executed when it is called.

Example:

# Correct Understanding (True Way)

A method should:

<li>Be defined inside a class.
<li>Have a return type (void, int, string, etc.).
<li>Have a method name.
<li>Use parentheses ().
<li>Contain code inside { }.

```csharp 
//  returnType MethodName(parameters)
// {
      // code
//   }

void GetFacultyDetails()
{
    // Code to accept details
}
 public void DisplayFacultyDetails()
    {
        Console.WriteLine("Hello from faculty");
    }
}
```

In the faculty program:

- `GetFacultyDetails()` accepts input from the user.
- `DisplayFacultyDetails()` displays the stored details.

### Calling Method
```csharp 
// in program.cs 
Student s = new Student();
s.Display();
```

### ❌ Wrong
```cshap
  class Student
  {
      public string Display;
  }
```

| Data Member    | Method           |
| -------------- | ---------------- |
| Stores data    | Performs action  |
| Variable       | Function         |
| `string Name;` | `void Display()` |
| Holds value    | Executes code    |


``` cshap
class Student
{
    public string Name;      // Data Member

    public void Display()    // Method
    {
        Console.WriteLine(Name);
    }
}
```

## Example of Common Mistake

```csharp
Faculty faculty;
faculty.DisplayFacultyDetails();
// Error: The object is declared but not instantiated.
```

Correct code:

```csharp
Faculty faculty = new Faculty();
faculty.DisplayFacultyDetails();
```
## Array of Objects

An array of objects is used when we want to store multiple records of the same type.

Real-time example:

A company does not store only one employee. It stores many employees. So, we can use an array of employee objects.

```csharp
Employee[] employees = new Employee[5];
```

This can store five employee records.



## Constructor

# Constructor Working Process

![Constructor Working Process](https://res.cloudinary.com/dpdxoxtog/image/upload/v1781164683/ChatGPT_Image_Jun_11_2026_01_27_46_PM_u9hkcc.png)

## Explanation

When an object is created using the `new` keyword:

1. Memory is allocated for the object.
2. The constructor is called automatically.
3. Data members are initialized.
4. The object becomes ready to use.

---

A constructor is a special method that runs automatically when an object is created.

Real-time example:

- When a new student admission form is opened, some details can be filled immediately at the time of creating the record. 
- In programming, a constructor helps initialize values when the object is created.

```csharp
class Student
{
    int id;

    public Student(int studentId)
    {
        id = studentId;
    }
}
```

Object creation:

```csharp
Student s1 = new Student(101);
```

Here, the value `101` is passed when the object is created.

---



# Types of Constructors in C#

## 1. Default Constructor

A default constructor does not take any parameter.

```csharp
class Student
{
    public Student()
    {
        Console.WriteLine("Default Constructor Called");
    }
}
```

## 2. Parameterized Constructor

A parameterized constructor takes values as parameters.

```csharp
class Student
{
    int id;

    public Student(int studentId)
    {
        id = studentId;
    }
}
```

## 3. Copy Constructor

A copy constructor copies values from one object to another object.

```csharp
class Student
{
    int id;

    public Student(int studentId)
    {
        id = studentId;
    }

    public Student(Student s)
    {
        id = s.id;
    }
}
```

## 4. Static Constructor

A static constructor is used to initialize static data. It runs only once automatically.

```csharp
class Demo
{
    static Student()
    {
        Console.WriteLine("Static Constructor Called");
    }
}
```

## 5. Private Constructor

A private constructor restricts object creation from outside the class.

```csharp
class Test
{
    private Student()
    {
    }
}
```

## Constructor Overloading

Constructor overloading means creating more than one constructor in the same class with different parameters.

```csharp
class Student
{
    public Student()
    {
    }

    public Student(string name)
    {
    }

    public Student(int id, string name)
    {
    }
}
```

## Constructor Comparison Table

| Constructor Type          | Parameters | Purpose                              |
| ------------------------- | ---------- | ------------------------------------ |
| Default Constructor       | No         | Initializes default values           |
| Parameterized Constructor | Yes        | Initializes user-given values        |
| Copy Constructor          | Object     | Copies values from another object    |
| Static Constructor        | No         | Initializes static data              |
| Private Constructor       | No         | Restricts object creation            |
| Constructor Overloading   | Different  | Provides multiple ways to initialize |

---

# Implementation for Program :

## Program 1:

Create a class named `Faculty` with ID, Name, Age, Weight, and Height as data members. Create member functions `GetFacultyDetails()` and `DisplayFacultyDetails()`.

### C# Program

```csharp
using System;

class Faculty
{
    // These variables store one faculty member's details.
    int id;
    string name;
    int age;
    double weight;
    double height;

    public void GetFacultyDetails()
    {
        // Take faculty details from the user.
        Console.Write("Enter Faculty ID: ");
        id = Convert.ToInt32(Console.ReadLine());

        Console.Write("Enter Faculty Name: ");
        name = Console.ReadLine();

        Console.Write("Enter Faculty Age: ");
        age = Convert.ToInt32(Console.ReadLine());

        Console.Write("Enter Faculty Weight: ");
        weight = Convert.ToDouble(Console.ReadLine());

        Console.Write("Enter Faculty Height: ");
        height = Convert.ToDouble(Console.ReadLine());
    }

    public void DisplayFacultyDetails()
    {
        // Display the stored faculty details.
        Console.WriteLine("\nFaculty Details");
        Console.WriteLine("ID: " + id);
        Console.WriteLine("Name: " + name);
        Console.WriteLine("Age: " + age);
        Console.WriteLine("Weight: " + weight + " kg");
        Console.WriteLine("Height: " + height + " cm");
    }
}

class Program
{
    static void Main()
    {
        // Create one faculty record and use its functions.
        Faculty faculty = new Faculty();

        faculty.GetFacultyDetails();
        faculty.DisplayFacultyDetails();
    }
}
```

### Explanation

Here, the program prepares one common format named `Faculty` for storing faculty details.

When `Faculty faculty = new Faculty();` runs, one actual faculty record is created in memory. After that:

- `GetFacultyDetails()` fills the record with values entered by the user.
- `DisplayFacultyDetails()` shows those stored values on the screen.

### Expected Output

```text
Enter Faculty ID: 101
Enter Faculty Name: Raj Mehta
Enter Faculty Age: 35
Enter Faculty Weight: 72
Enter Faculty Height: 175

Faculty Details
ID: 101
Name: Raj Mehta
Age: 35
Weight: 72 kg
Height: 175 cm
```

---

## Program 3:

Write a program to calculate Volume of a Cube using constructor.

### C# Program

```csharp
using System;

class Cube
{
    private double side;
    private double volume;

    // Constructor
    public Cube(double s)
    {
        side = s;
        volume = side * side * side;
    }

    public void DisplayVolume()
    {
        Console.WriteLine("Side of Cube: " + side);
        Console.WriteLine("Volume of Cube: " + volume);
    }
}

class Program
{
    static void Main(string[] args)
    {
        Console.Write("Enter the side of the cube: ");
        double side = Convert.ToDouble(Console.ReadLine());

        Cube c = new Cube(side);
        c.DisplayVolume();

        Console.ReadKey();
    }
}
```

## Sample Output

```text
Enter the side of the cube: 5
Side of Cube: 5
Volume of Cube: 125
```


---

# Common Errors and Troubleshooting

| Error                     | Reason                               | Solution                               |
| ------------------------- | ------------------------------------ | -------------------------------------- |
| NullReferenceException    | Object not created                   | Create object using `new` keyword      |
| FormatException           | Invalid input type entered           | Enter correct numeric or string values |
| IndexOutOfRangeException  | Accessing invalid array index        | Check array size and index             |
| Constructor not found     | Incorrect constructor call           | Match constructor parameters correctly |
| Object reference required | Using non-static members incorrectly | Create object before accessing members |



---
# Lab Tasks:
1. Write a program to create a class named Faculty with ID, Name, Age, Weight and Height as data members & also create a member function like GetFacultyDetails() and DisplayFacultyDetails().

2. Write a program to create a class named Employee with Emp_ID, Name, Department, Designation and Salary as data members & also create a member function like GetEmpDetails() and DisplayEmpDetails() for five different employee objects.

3. Write a program to calculate Volume of a Cube using constructor.

4. A car dealership wants to display details of premium sports cars. Create a Car object with Make, Model, Year, FuelType and Horsepower details (use constructor).

5. Write a program to manage a supermarket inventory. Create an Item class with Item_Code, Item_Name, and Stock_Quantity. Use a constructor to initialize the objects and an array of objects to store and display the details of 10 different items.

6. A clinic management system needs to store patient records. Create a program with a Patient class containing Patient_ID, Name, Age, and Disease. Use an array of objects to accept details for multiple patients and display a formatted summary report of all patients.

---
