# LINQ Practice Questions

Practice LINQ Method Syntax using the following operators only:

- Select
- SelectMany
- Where
- OfType
- Take
- TakeWhile
- Skip
- SkipWhile
- Distinct
- First
- FirstOrDefault
- Single
- SingleOrDefault
- Any
- All
- Contains

---

# Data Setup

Use the following classes and sample data for all questions.

```csharp
class Department
{
    public int DeptId { get; set; }
    public string DeptName { get; set; }
}

class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
    public decimal Salary { get; set; }
    public int DeptId { get; set; }
    public List<string> Skills { get; set; }
}

var departments = new List<Department>
{
    new Department { DeptId = 1, DeptName = "HR" },
    new Department { DeptId = 2, DeptName = "IT" },
    new Department { DeptId = 3, DeptName = "Finance" },
    new Department { DeptId = 4, DeptName = "Marketing" }
};

var employees = new List<Employee>
{
    new Employee { Id = 101, Name = "Amit",   Age = 28, Salary = 75000, DeptId = 2, Skills = new List<string>{ "C#", "SQL", "Angular" } },
    new Employee { Id = 102, Name = "Neha",   Age = 34, Salary = 95000, DeptId = 2, Skills = new List<string>{ "Java", "C#", "React" } },
    new Employee { Id = 103, Name = "Raj",    Age = 45, Salary = 60000, DeptId = 1, Skills = new List<string>{ "Excel", "Communication" } },
    new Employee { Id = 104, Name = "Priya",  Age = 29, Salary = 82000, DeptId = 3, Skills = new List<string>{ "Accounting", "SQL" } },
    new Employee { Id = 105, Name = "Karan",  Age = 31, Salary = 88000, DeptId = 2, Skills = new List<string>{ "C#", "Azure", "Docker" } },
    new Employee { Id = 106, Name = "Simran", Age = 26, Salary = 72000, DeptId = 4, Skills = new List<string>{ "Design", "Photoshop" } },
    new Employee { Id = 107, Name = "Rohan",  Age = 38, Salary = 92000, DeptId = 5, Skills = new List<string>{ "Salesforce", "Communication", "Excel" } },
    new Employee { Id = 108, Name = "Sneha",  Age = 27, Salary = 68000, DeptId = 1, Skills = new List<string>{ "Recruitment", "Communication", "Excel" } },
    new Employee { Id = 109, Name = "Vikram", Age = 40, Salary = 98000, DeptId = 3, Skills = new List<string>{ "Accounting", "Power BI", "SQL" } },
    new Employee { Id = 110, Name = "Pooja",  Age = 30, Salary = 85000, DeptId = 4, Skills = new List<string>{ "Canva", "Photoshop", "Marketing" } }
};
```

---

# Practice Questions

1. Display the names of all employees.
2. Display only employee IDs.
3. Display Name and Salary.
4. Display Name and Age.
5. Create an anonymous object containing Name and Department Id.
6. Display employees older than 30 years.
7. Display employees whose salary is greater than ₹80,000.
8. Display employees belonging to the IT department.
9. Display employees belonging to the Finance department.
10. Display employees whose name starts with 'A'.
11. Display employees whose name ends with 'a'.
12. Display employees whose age is between 25 and 35 years.
13. Display every skill of every employee.
14. Display all unique skills.
15. Display employees who know the "C#" skill.
16. Display all distinct Department IDs.
17. Display all distinct employee ages.
18. Display the first employee.
19. Display the first three employees.
20. Skip the first employee and display the remaining employees.
21. Skip the first three employees and display the remaining employees.
22. Find the first employee from the IT department.
23. Find the employee with Id = 999 using `FirstOrDefault()`.
24. Check whether any employee earns more than ₹90,000.
25. Check whether the "Docker" skill exists in the company.
26. Display employee Name and Annual Salary.
27. Display employee Name and total number of Skills.
28. Display employees whose salary is between ₹70,000 and ₹90,000.
29. Display employees who know the "SQL" skill.
30. Display employees who belong to the IT department and earn more than ₹80,000.
31. Display all skills of employees earning more than ₹80,000.
32. Display all skills of employees working in the IT department.
33. From the following mixed collection, retrieve only Employee objects.

```csharp
List<object> mixed = new List<object>
{
    employees[0],
    "Hello",
    employees[1],
    500,
    employees[2],
    true
};
```

34. From the above mixed collection, retrieve only string values.
35. Assuming employees are sorted by Age, return employees until Age becomes 32 or above.
36. Skip employees while Salary is greater than or equal to ₹80,000.
37. Retrieve the employee whose Id is 103 using `Single()`.
38. Retrieve the employee whose Id is 999 using `SingleOrDefault()`.
39. Verify whether every employee has at least one skill.
40. Verify whether the "React" skill exists in the company.
41. Display names of employees older than 30 years.
42. Display unique skills of employees earning more than ₹80,000.
43. Display names of employees who know the "SQL" skill.
44. Display the first employee who knows the "Azure" skill.
45. Check whether every IT employee earns more than ₹70,000.
46. Check whether any Finance employee knows the "SQL" skill.
47. Display the first two employee names.
48. Skip the first two employees and display only their names.
49. Display unique skills of employees whose age is greater than 30 years.
50. Check whether every employee knows either the "C#" skill or the "SQL" skill.

---

# LINQ Printing Guide

| Result Type | How to Print |
|--------------|--------------|
| Collection / List | Convert the result to a List using `.ToList()` and print using a `foreach` loop. |
| Filtered Collection | Use `.Where(...).ToList()` and print using a `foreach` loop. |
| Flattened Collection | Use `.SelectMany(...).ToList()` and print using a `foreach` loop. |
| Distinct Collection | Use `.Distinct().ToList()` and print using a `foreach` loop. |
| Single Object | Print object properties using `Console.WriteLine()`. |
| Nullable Object | Check for `null` before printing the object properties. |
| Boolean Result | Print the boolean value directly using `Console.WriteLine()`. |
