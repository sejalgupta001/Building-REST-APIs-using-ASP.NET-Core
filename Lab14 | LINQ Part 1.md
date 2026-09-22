# LINQ Lab-1 Explanation

> Theory, projection operators, and filtering operators with simple input and output.

---

# 1. What is LINQ?

LINQ stands for:

# Language Integrated Query

LINQ is a C# feature used to query data from collections, databases, XML, and objects using one common syntax.

LINQ helps us:

- Filter data
- Select specific fields
- Sort data
- Group data
- Join data
- Calculate values

---

# 2. Why LINQ is Useful

Without LINQ, filtering data needs loops and extra code.

```csharp
List<int> numbers = new List<int>() { 1, 2, 3, 4, 5, 6 };

List<int> result = new List<int>();

foreach(var n in numbers)
{
    if(n > 3)
    {
        result.Add(n);
    }
}
```

Output:

```text
4
5
6
```

Using LINQ:

```csharp
var result = numbers.Where(n => n > 3);
```

Output:

```text
4
5
6
```

LINQ makes code shorter, cleaner, and easier to read.

---

# 3. LINQ Flow

```text
Data Source -> Query -> Execution -> Result
```

Example:

```csharp
List<int> numbers = new List<int>() { 1, 2, 3, 4, 5, 6 };

var result = numbers
                .Where(n => n > 3)
                .Select(n => n * 2);
```

Step-by-step:

```text
Input:  1, 2, 3, 4, 5, 6
Filter: 4, 5, 6
Select: 8, 10, 12
```

Output:

```text
8
10
12
```

---

# 4. Query Syntax vs Method Syntax

LINQ supports two common styles.

# Query Syntax

```csharp
var result = from s in students
             where s.Branch == "CE"
             select s.Name;
```

# Method Syntax

```csharp
var result = students
                .Where(s => s.Branch == "CE")
                .Select(s => s.Name);
```

Both examples return the same output:

```text
Amit
Raj
```

Method syntax is commonly used in real projects because it works well with all LINQ operators.

---

# 5. Lambda Expressions

A **lambda expression** is a short function that you write directly inside your code.

It has the following syntax:

```text
input => output
```

You can read it as:

> **"input goes to output."**

---

## Lambda Expression = Short Anonymous Function

Example:

```csharp
s => s.Name
```

Meaning:

- Take each `s` (student).
- Return `s.Name`.

It is simply a shorter version of writing a normal method.

```csharp
string GetName(Student s)
{
    return s.Name;
}
```

Instead of creating a separate method, we write the function directly where it is needed.

---

## Lambda Syntax

### Single Parameter

```csharp
x => x * 2
```

Meaning:

- Take `x`
- Return `x * 2`

---

### Multiple Parameters

When there are multiple parameters, use parentheses.

```csharp
(x, y) => x + y
```

Meaning:

- Take `x` and `y`
- Return their sum.

---

### Multiple Statements

If the logic is larger than one expression, use curly braces.

```csharp
s =>
{
    var result = s.CPI * 10;
    return result;
}
```

---

## Why Do We Use Lambda Expressions in LINQ?

LINQ methods such as `Where()`, `Select()`, `Any()`, and `OrderBy()` expect a function.

A lambda expression is the easiest way to provide that function.

Example:

```csharp
students.Where(s => s.CPI > 8);
```

Here,

- `s` represents each student.
- `s.CPI > 8` is the condition.
- LINQ checks every student using this condition and returns only those whose CPI is greater than 8.

---

## Common Lambda Examples

| Operation | Lambda |
|-----------|--------|
| Select student names | `s => s.Name` |
| Filter CE students | `s => s.Branch == "CE"` |
| Filter students with CPI greater than 9 | `s => s.CPI > 9` |
| Check if any student is in Semester 5 | `s => s.Sem == 5` |
| Get student courses | `s => s.Courses` |

---

## Lambda with LINQ Method Syntax

LINQ Method Syntax uses methods like `Where()`, `Select()`, and `Any()`.

The function passed to these methods is written using a lambda expression.

```csharp
var result = students
                .Where(s => s.Age > 18)
                .Select(s => s.Name);
```

Explanation:

- `Where(s => s.Age > 18)` filters students whose age is greater than 18.
- `Select(s => s.Name)` returns only the student names.
- The final result contains the names of students older than 18.

---

# 6. IEnumerable vs IQueryable

```text
IEnumerable:

DB -> Fetch All Records -> Filter Records In-Memory -> Result
```

---

```text
IQueryable:

DB -> Filter Records Based On Query -> Fetch Only Needed Records -> Result
```

| Feature    | IEnumerable                     | IQueryable                              |
| ---------- | ------------------------------- | --------------------------------------- |
| Works with | In-memory data                  | Database query                          |
| Namespace  | System.Collections.Generic      | System.Linq                             |
| Execution  | Data is usually already fetched | Query is built and executed by provider |
| Best for   | List, Array, Dictionary         | Entity Framework, database tables       |

# IEnumerable Example

```csharp
IEnumerable<Student> students = context.Students.ToList();

var result = students.Where(s => s.Branch == "CE");
```

Here, records are fetched first and filtering happens in memory.

# IQueryable Example

```csharp
IQueryable<Student> students = context.Students;

var result = students.Where(s => s.Branch == "CE");
```

Here, filtering can be converted into SQL and executed in the database.

---

# Required Namespace

```csharp
using System.Linq;
using System.Collections;
using System.Collections.Generic;
```

---

# Required Packages for Project

```csharp
Microsoft.EntityFrameworkCore
Microsoft.EntityFrameworkCore.SqlServer
Microsoft.EntityFrameworkCore.Tools
Microsoft.EntityFrameworkCore.Design
```

---

# 7. Sample Data

Use this same data for all Lab 7 examples. This list contains 60 student records.

```csharp
public class Student
{
    public int Rno { get; set; }
    public string Name { get; set; }
    public string Branch { get; set; }
    public int Sem { get; set; }
    public double CPI { get; set; }
    public object ExtraValue { get; set; }
    public List<string> Courses { get; set; }
}
```

```csharp
var sampleData = new[]
{
    new { Name = "Amit", Branch = "CE", Sem = 3, CPI = 8.5, ExtraValue = (object)"Topper", Courses = new List<string>() { "C#", "DBMS" } },
    new { Name = "Neha", Branch = "IT", Sem = 4, CPI = 9.1, ExtraValue = (object)100, Courses = new List<string>() { "Java", "AI" } },
    new { Name = "Raj", Branch = "CE", Sem = 3, CPI = 7.8, ExtraValue = (object)"Sports", Courses = new List<string>() { "C#", "Math" } },
    new { Name = "Priya", Branch = "IT", Sem = 5, CPI = 8.9, ExtraValue = (object)200, Courses = new List<string>() { "Python", "DBMS" } },
    new { Name = "Kiran", Branch = "ME", Sem = 2, CPI = 7.2, ExtraValue = (object)"Workshop", Courses = new List<string>() { "CAD", "Physics" } },
    new { Name = "Pooja", Branch = "CE", Sem = 4, CPI = 8.3, ExtraValue = (object)150, Courses = new List<string>() { "C#", "Data Structures" } },
    new { Name = "Rahul", Branch = "EC", Sem = 6, CPI = 7.9, ExtraValue = (object)"Robotics", Courses = new List<string>() { "Signals", "IoT" } },
    new { Name = "Sneha", Branch = "IT", Sem = 3, CPI = 8.7, ExtraValue = (object)"Hackathon", Courses = new List<string>() { "Python", "Web" } },
    new { Name = "Vivek", Branch = "CE", Sem = 5, CPI = 6.9, ExtraValue = (object)75, Courses = new List<string>() { "JavaScript", "DBMS" } },
    new { Name = "Anjali", Branch = "ME", Sem = 4, CPI = 8.1, ExtraValue = (object)"Design", Courses = new List<string>() { "CAD", "Thermodynamics" } },
    new { Name = "Manish", Branch = "EC", Sem = 2, CPI = 7.5, ExtraValue = (object)120, Courses = new List<string>() { "Electronics", "Math" } },
    new { Name = "Riya", Branch = "CE", Sem = 6, CPI = 9.3, ExtraValue = (object)"Research", Courses = new List<string>() { "C#", "AI" } },
    new { Name = "Harsh", Branch = "IT", Sem = 5, CPI = 7.4, ExtraValue = (object)"NSS", Courses = new List<string>() { "Java", "Networking" } },
    new { Name = "Meera", Branch = "CE", Sem = 2, CPI = 8.0, ExtraValue = (object)180, Courses = new List<string>() { "C#", "Physics" } },
    new { Name = "Sahil", Branch = "ME", Sem = 6, CPI = 6.8, ExtraValue = (object)"Sports", Courses = new List<string>() { "Mechanics", "CAD" } },
    new { Name = "Nisha", Branch = "EC", Sem = 4, CPI = 8.6, ExtraValue = (object)90, Courses = new List<string>() { "IoT", "Signals" } },
    new { Name = "Arjun", Branch = "IT", Sem = 2, CPI = 7.7, ExtraValue = (object)"Coding", Courses = new List<string>() { "Python", "DBMS" } },
    new { Name = "Divya", Branch = "CE", Sem = 5, CPI = 8.8, ExtraValue = (object)250, Courses = new List<string>() { "C#", "Web" } },
    new { Name = "Yash", Branch = "EC", Sem = 3, CPI = 7.1, ExtraValue = (object)"Music", Courses = new List<string>() { "Electronics", "C" } },
    new { Name = "Kavya", Branch = "ME", Sem = 5, CPI = 8.4, ExtraValue = (object)300, Courses = new List<string>() { "Thermodynamics", "Math" } },
    new { Name = "Rohan", Branch = "CE", Sem = 4, CPI = 6.7, ExtraValue = (object)"Volunteer", Courses = new List<string>() { "Data Structures", "DBMS" } },
    new { Name = "Isha", Branch = "IT", Sem = 6, CPI = 9.0, ExtraValue = (object)"Topper", Courses = new List<string>() { "AI", "Python" } },
    new { Name = "Nikhil", Branch = "EC", Sem = 5, CPI = 7.6, ExtraValue = (object)60, Courses = new List<string>() { "Signals", "Networking" } },
    new { Name = "Tanya", Branch = "CE", Sem = 3, CPI = 8.2, ExtraValue = (object)"Seminar", Courses = new List<string>() { "C#", "Math" } },
    new { Name = "Om", Branch = "ME", Sem = 2, CPI = 7.0, ExtraValue = (object)110, Courses = new List<string>() { "Physics", "Mechanics" } },
    new { Name = "Bhavya", Branch = "IT", Sem = 4, CPI = 8.5, ExtraValue = (object)"Project", Courses = new List<string>() { "Java", "Web" } },
    new { Name = "Dev", Branch = "CE", Sem = 6, CPI = 9.2, ExtraValue = (object)400, Courses = new List<string>() { "AI", "DBMS" } },
    new { Name = "Jinal", Branch = "EC", Sem = 2, CPI = 7.3, ExtraValue = (object)"Club", Courses = new List<string>() { "C", "Electronics" } },
    new { Name = "Parth", Branch = "ME", Sem = 3, CPI = 8.1, ExtraValue = (object)210, Courses = new List<string>() { "CAD", "Math" } },
    new { Name = "Krisha", Branch = "IT", Sem = 5, CPI = 8.9, ExtraValue = (object)"Internship", Courses = new List<string>() { "Python", "Networking" } },
    new { Name = "Mihir", Branch = "CE", Sem = 2, CPI = 7.8, ExtraValue = (object)95, Courses = new List<string>() { "C#", "Physics" } },
    new { Name = "Avni", Branch = "EC", Sem = 6, CPI = 8.0, ExtraValue = (object)"Robotics", Courses = new List<string>() { "IoT", "Signals" } },
    new { Name = "Het", Branch = "ME", Sem = 4, CPI = 6.6, ExtraValue = (object)"Workshop", Courses = new List<string>() { "Mechanics", "CAD" } },
    new { Name = "Esha", Branch = "IT", Sem = 3, CPI = 8.3, ExtraValue = (object)170, Courses = new List<string>() { "JavaScript", "DBMS" } },
    new { Name = "Jay", Branch = "CE", Sem = 5, CPI = 7.5, ExtraValue = (object)"Sports", Courses = new List<string>() { "C#", "Web" } },
    new { Name = "Mansi", Branch = "EC", Sem = 4, CPI = 9.1, ExtraValue = (object)500, Courses = new List<string>() { "AI", "Electronics" } },
    new { Name = "Darshan", Branch = "ME", Sem = 6, CPI = 7.9, ExtraValue = (object)"Design", Courses = new List<string>() { "Thermodynamics", "CAD" } },
    new { Name = "Aarohi", Branch = "CE", Sem = 4, CPI = 8.7, ExtraValue = (object)225, Courses = new List<string>() { "Data Structures", "C#" } },
    new { Name = "Smit", Branch = "IT", Sem = 2, CPI = 6.9, ExtraValue = (object)"Music", Courses = new List<string>() { "Java", "Math" } },
    new { Name = "Falguni", Branch = "EC", Sem = 5, CPI = 8.2, ExtraValue = (object)"Seminar", Courses = new List<string>() { "Signals", "IoT" } },
    new { Name = "Meet", Branch = "CE", Sem = 3, CPI = 7.4, ExtraValue = (object)130, Courses = new List<string>() { "DBMS", "Web" } },
    new { Name = "Charmi", Branch = "ME", Sem = 5, CPI = 8.6, ExtraValue = (object)"Project", Courses = new List<string>() { "CAD", "Mechanics" } },
    new { Name = "Hiren", Branch = "IT", Sem = 6, CPI = 7.8, ExtraValue = (object)85, Courses = new List<string>() { "Networking", "AI" } },
    new { Name = "Kinjal", Branch = "CE", Sem = 2, CPI = 9.0, ExtraValue = (object)"Topper", Courses = new List<string>() { "C#", "Physics" } },
    new { Name = "Varun", Branch = "EC", Sem = 3, CPI = 7.2, ExtraValue = (object)"NCC", Courses = new List<string>() { "Electronics", "Math" } },
    new { Name = "Pallavi", Branch = "ME", Sem = 4, CPI = 8.0, ExtraValue = (object)145, Courses = new List<string>() { "Thermodynamics", "Physics" } },
    new { Name = "Naitik", Branch = "IT", Sem = 5, CPI = 8.4, ExtraValue = (object)"Coding", Courses = new List<string>() { "Python", "Web" } },
    new { Name = "Shreya", Branch = "CE", Sem = 6, CPI = 9.4, ExtraValue = (object)550, Courses = new List<string>() { "AI", "C#" } },
    new { Name = "Rudra", Branch = "EC", Sem = 2, CPI = 6.8, ExtraValue = (object)"Sports", Courses = new List<string>() { "C", "Electronics" } },
    new { Name = "Aditi", Branch = "ME", Sem = 3, CPI = 7.7, ExtraValue = (object)"Workshop", Courses = new List<string>() { "CAD", "Math" } },
    new { Name = "Kartik", Branch = "CE", Sem = 5, CPI = 8.1, ExtraValue = (object)190, Courses = new List<string>() { "DBMS", "Data Structures" } },
    new { Name = "Jiya", Branch = "IT", Sem = 4, CPI = 9.2, ExtraValue = (object)"Research", Courses = new List<string>() { "AI", "Python" } },
    new { Name = "Pranav", Branch = "EC", Sem = 6, CPI = 7.5, ExtraValue = (object)105, Courses = new List<string>() { "Signals", "Networking" } },
    new { Name = "Simran", Branch = "ME", Sem = 2, CPI = 8.3, ExtraValue = (object)"Design", Courses = new List<string>() { "Mechanics", "Physics" } },
    new { Name = "Dhruv", Branch = "CE", Sem = 4, CPI = 7.0, ExtraValue = (object)"Club", Courses = new List<string>() { "C#", "Math" } },
    new { Name = "Riddhi", Branch = "IT", Sem = 3, CPI = 8.6, ExtraValue = (object)230, Courses = new List<string>() { "Java", "DBMS" } },
    new { Name = "Ankit", Branch = "EC", Sem = 5, CPI = 8.8, ExtraValue = (object)"Robotics", Courses = new List<string>() { "IoT", "AI" } },
    new { Name = "Payal", Branch = "ME", Sem = 6, CPI = 7.1, ExtraValue = (object)70, Courses = new List<string>() { "Thermodynamics", "CAD" } },
    new { Name = "Moksh", Branch = "CE", Sem = 2, CPI = 7.9, ExtraValue = (object)"Seminar", Courses = new List<string>() { "Web", "C#" } },
    new { Name = "Sakshi", Branch = "IT", Sem = 6, CPI = 8.5, ExtraValue = (object)"Internship", Courses = new List<string>() { "Networking", "Python" } }
};

List<Student> students = sampleData
    .Select((s, index) => new Student()
    {
        Rno = index + 1,
        Name = s.Name,
        Branch = s.Branch,
        Sem = s.Sem,
        CPI = s.CPI,
        ExtraValue = s.ExtraValue,
        Courses = s.Courses
    })
    .ToList();
```

Input table preview:

| Rno | Name  | Branch | Sem | CPI | ExtraValue | Courses      |
| --- | ----- | ------ | --- | --- | ---------- | ------------ |
| 1   | Amit  | CE     | 3   | 8.5 | Topper     | C#, DBMS     |
| 2   | Neha  | IT     | 4   | 9.1 | 100        | Java, AI     |
| 3   | Raj   | CE     | 3   | 7.8 | Sports     | C#, Math     |
| 4   | Priya | IT     | 5   | 8.9 | 200        | Python, DBMS |
| 5   | Kiran | ME     | 2   | 7.2 | Workshop   | CAD, Physics |
| 6   | Pooja | CE     | 4   | 8.3 | 150        | C#, Data Structures |
| 7   | Rahul | EC     | 6   | 7.9 | Robotics   | Signals, IoT |
| 8   | Sneha | IT     | 3   | 8.7 | Hackathon  | Python, Web |
| 9   | Vivek | CE     | 5   | 6.9 | 75         | JavaScript, DBMS |
| 10  | Anjali | ME    | 4   | 8.1 | Design     | CAD, Thermodynamics |

The code above contains 60 records. The examples below show short output using the first few matching records to keep the explanation readable.

---

# 8. Projection Operators

Projection operators are used to select or transform data.

Projection operators covered:

1. Select
2. SelectMany

---

# 8.1 Select Operator

`Select` is used to select one field or create a new shape from each record.

# Example 1: Select Only Names

```csharp
var result = students.Select(s => s.Name);
```

Output:

```text
Amit
Neha
Raj
Priya
```

# Example 2: Select Name and CPI

```csharp
var result = students.Select(s => new
{
    s.Name,
    s.CPI
});
```

Output:

```text
Amit - 8.5
Neha - 9.1
Raj - 7.8
Priya - 8.9
```

---

# 8.2 SelectMany Operator

`SelectMany` is used to flatten nested collections into one single collection.

# Example: Get All Courses

Input data used for this example:

| Name  | Courses      |
| ----- | ------------ |
| Amit  | C#, DBMS     |
| Neha  | Java, AI     |
| Raj   | C#, Math     |
| Priya | Python, DBMS |

Here, `SelectMany` is applied on the `Courses` list of each student.

```csharp
var result = students.SelectMany(s => s.Courses);
```

Output:

```text
C#
DBMS
Java
AI
C#
Math
Python
DBMS
```

# Select vs SelectMany

Using `Select`:

```csharp
var result = students.Select(s => s.Courses);
```

Output structure:

```text
[C#, DBMS]
[Java, AI]
[C#, Math]
[Python, DBMS]
```

Using `SelectMany`:

```csharp
var result = students.SelectMany(s => s.Courses);
```

Output structure:

```text
C#, DBMS, Java, AI, C#, Math, Python, DBMS
```

---

# 9. Filtering Operators

Filtering operators are used to get only the required data from a collection.

Filtering operators summary:

| Operator        | Purpose                                   |
| --------------- | ----------------------------------------- |
| Where           | Filter records by condition               |
| OfType          | Filter values by type                     |
| Take            | Return first N records                    |
| TakeWhile       | Return records while condition is true    |
| Skip            | Ignore first N records                    |
| SkipWhile       | Skip records while condition is true      |
| Distinct        | Remove duplicate values                   |
| First           | Return first record, exception if missing |
| FirstOrDefault  | Return first record or default value      |
| Single          | Return exactly one record                 |
| SingleOrDefault | Return one record or default value        |
| Any             | Check at least one match                  |
| All             | Check all records                         |
| Contains        | Check whether value exists                |

---

# 9.1 Where Operator

`Where` filters data based on a condition.

```csharp
var result = students.Where(s => s.Branch == "CE");
```

Output:

```text
Amit
Raj
```

---

# 9.2 OfType Operator

`OfType` filters elements based on data type.

Input:

```csharp
ArrayList values = new ArrayList()
{
    "Amit",
    10,
    "Neha",
    20,
    "Raj"
};
```

Example:

```csharp
var result = values.OfType<string>();
```

Output:

```text
Amit
Neha
Raj
```

---

# 9.3 Take Operator

`Take` returns the first N records.

```csharp
var result = students.Take(2);
```

Output:

```text
Amit
Neha
```

---

# 9.4 TakeWhile Operator

`TakeWhile` returns records while the condition is true. It stops when the first false condition is found.

```csharp
var result = students.TakeWhile(s => s.CPI >= 8);
```

Output:

```text
Amit
Neha
```

Raj has CPI `7.8`, so `TakeWhile` stops there and does not check Priya.

---

# 9.5 Skip Operator

`Skip` ignores the first N records.

```csharp
var result = students.Skip(2);
```

Output:

```text
Raj
Priya
```

---

# 9.6 SkipWhile Operator

`SkipWhile` skips records while the condition is true. It starts returning records after the first false condition is found.

```csharp
var result = students.SkipWhile(s => s.CPI >= 8);
```

Output:

```text
Raj
Priya
```

Amit and Neha are skipped because their CPI is greater than or equal to 8. Raj fails the condition, so Raj and the remaining records are returned.

---

# 9.7 Distinct Operator

`Distinct` removes duplicate values.

Example:

```csharp
var result = students
                .Select(s => s.Branch)
                .Distinct();
```

Output:

```text
CE
IT
```

---

# 9.8 First Operator

`First` returns the first record. It throws an exception if no record is found.

```csharp
var result = students.First();
```

Output:

```text
Amit
```

With condition:

```csharp
var result = students.First(s => s.Branch == "IT");
```

Output:

```text
Neha
```

---

# 9.9 FirstOrDefault Operator

`FirstOrDefault` returns the first matching record. If no record is found, it returns `null` for objects.

```csharp
var result = students.FirstOrDefault(s => s.Branch == "ME");
```

Output:

```text
null
```

---

# 9.10 Single Operator

`Single` returns exactly one matching record. It throws an exception if no record is found or more than one record is found.

```csharp
var result = students.Single(s => s.Rno == 2);
```

Output:

```text
Neha
```

Unsafe example:

```csharp
var result = students.Single(s => s.Branch == "CE");
```

Output:

```text
Exception: Sequence contains more than one matching element
```

---

# 9.11 SingleOrDefault Operator

`SingleOrDefault` returns exactly one matching record. If no record is found, it returns `null`. If more than one record is found, it still throws an exception.

```csharp
var result = students.SingleOrDefault(s => s.Rno == 5);
```

Output:

```text
null
```

Example with one matching record:

```csharp
var result = students.SingleOrDefault(s => s.Rno == 4);
```

Output:

```text
Priya
```

---

# 9.12 Any Operator

`Any` checks whether at least one record exists or matches a condition.

```csharp
bool result = students.Any(s => s.CPI > 9);
```

Output:

```text
True
```

---

# 9.13 All Operator

`All` checks whether all records satisfy a condition.

```csharp
bool result = students.All(s => s.CPI >= 7);
```

Output:

```text
True
```

Another example:

```csharp
bool result = students.All(s => s.Branch == "CE");
```

Output:

```text
False
```

---

# 9.14 Contains Operator

`Contains` checks whether a collection contains a specific value.

Input:

```csharp
List<string> branches = students
                            .Select(s => s.Branch)
                            .ToList();
```

Example:

```csharp
bool result = branches.Contains("IT");
```

Output:

```text
True
```

Another example:

```csharp
bool result = branches.Contains("ME");
```

Output:

```text
False
```

---

# 10. LINQ Quick Revision Table

This table provides a quick comparison of all LINQ operators covered in this lab.

| LINQ Method | Purpose | Example Code | SQL Equivalent | Output Meaning |
| ------------ | ------- | ------------ | -------------- | -------------- |
| **Where** | Filter records based on a condition | `students.Where(s => s.Branch == "CE")` | `SELECT * FROM Students WHERE Branch = 'CE'` | Returns only CE students |
| **Select** | Select specific fields or transform data | `students.Select(s => s.Name)` | `SELECT Name FROM Students` | Returns all student names |
| **SelectMany** | Flatten nested collections into one list | `students.SelectMany(s => s.Courses)` | `SELECT Course FROM StudentCourses` | Returns all courses from all students in one list |
| **OfType** | Filter elements by data type | `values.OfType<string>()` | *No direct SQL equivalent* | Returns only string values |
| **Take** | Return the first N records | `students.Take(2)` | `SELECT TOP 2 * FROM Students` | Returns the first 2 students |
| **TakeWhile** | Return records while the condition is true | `students.TakeWhile(s => s.CPI >= 8)` | *No direct SQL equivalent* | Stops when the first false condition is found |
| **Skip** | Skip the first N records | `students.Skip(2)` | `SELECT * FROM Students ORDER BY Rno OFFSET 2 ROWS` | Ignores the first 2 students |
| **SkipWhile** | Skip records while the condition is true | `students.SkipWhile(s => s.CPI >= 8)` | *No direct SQL equivalent* | Starts returning records after the first false condition |
| **Distinct** | Remove duplicate values | `students.Select(s => s.Branch).Distinct()` | `SELECT DISTINCT Branch FROM Students` | Returns unique branch names |
| **First** | Return the first record | `students.First()` | `SELECT TOP 1 * FROM Students` | Returns the first record or throws an exception if the sequence is empty |
| **FirstOrDefault** | Return the first record or the default value | `students.FirstOrDefault()` | `SELECT TOP 1 * FROM Students` | Returns the first record or `null` if no record exists |
| **Single** | Return exactly one matching record | `students.Single(s => s.Rno == 2)` | `SELECT TOP 2 * FROM Students WHERE Rno = 2` | Returns one record or throws an exception if zero or multiple records are found |
| **SingleOrDefault** | Return one matching record or the default value | `students.SingleOrDefault(s => s.Rno == 2)` | `SELECT TOP 2 * FROM Students WHERE Rno = 2` | Returns one record or `null`; throws an exception if multiple records are found |
| **Any** | Check whether at least one record matches | `students.Any(s => s.CPI > 9)` | `EXISTS(...)` | Returns `true` if at least one matching record exists |
| **All** | Check whether all records satisfy a condition | `students.All(s => s.CPI >= 7)` | *No direct SQL equivalent* | Returns `true` only if every record satisfies the condition |
| **Contains** | Check whether a value exists in a collection | `branches.Contains("IT")` | `SELECT * FROM Students WHERE Branch IN ('IT')` | Returns `true` if the value exists; otherwise `false` |

> **Notes**
>
> - `Where`, `Select`, `Distinct`, `Take`, and `Skip` have close SQL equivalents.
> - `TakeWhile`, `SkipWhile`, and `All` work on sequence order, so they do not have direct SQL equivalents.
> - `Any()` is conceptually similar to SQL's `EXISTS`.
> - `First()` and `Single()` throw exceptions when their conditions are not satisfied.
> - `FirstOrDefault()` and `SingleOrDefault()` return the default value (`null` for reference types) when no matching record is found.
