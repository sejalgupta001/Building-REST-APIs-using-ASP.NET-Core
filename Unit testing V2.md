# Unit Testing with xUnit and Moq

## 1. Simple Understanding

### What is Unit Testing?

**Unit Testing** means testing a small part of an application, usually a single method or class, separately.

For example:

```text
StudentService
     ↓
   Add()
     ↓
 Test whether Add() works correctly
```

Instead of running the complete application, we directly test the method.

### Why Do We Need Unit Testing?

Suppose we have:

```text
Controller → Service → Database
```

If the `StudentService.Add()` method has a bug, we want to find it quickly without running the complete application.

Unit testing helps us:

* Find bugs early
* Test individual methods
* Make changes safely
* Automatically check whether code is working

---

### Application Project and Test Project

Usually, we keep tests in a separate project.

```text
Solution
│
├── StudentApi                         ← MAIN APPLICATION PROJECT
│   │
│   ├── Models
│   │   └── Student.cs
│   │
│   ├── Services
│   │   ├── IStudentService.cs
│   │   └── StudentService.cs
│   │
│   └── Controllers
│       └── StudentController.cs
│
└── StudentApi.Tests                   ← TEST PROJECT
    │
    ├── StudentServiceTests.cs
    ├── StudentTestData.cs
    └── StudentControllerTests.cs
```

* **StudentApi** → Actual application
* **StudentApi.Tests** → Unit tests

---

### Our First Example

We will first create a simple service.
Create this file in: StudentApi → Models → Student.cs
```csharp
public class Student
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public int Marks { get; set; }
}
```

Now create the service:
Create this file in: StudentApi → Services → StudentService.cs
```csharp
public class StudentService
{
    private readonly List<Student> _students = new();

    public void Add(Student student)
    {
        if (string.IsNullOrWhiteSpace(student.Name))
            throw new ArgumentException("Name is required.");

        if (student.Marks < 0 || student.Marks > 100)
            throw new ArgumentException("Marks must be between 0 and 100.");

        _students.Add(student);
    }

    public Student? GetById(int id)
    {
        return _students.FirstOrDefault(x => x.Id == id);
    }

    public int GetCount()
    {
        return _students.Count;
    }
}
```

We will now test the `Add()` method.

---

# 2. xUnit Test

## What is xUnit?

**xUnit** is a testing framework for .NET applications.

It provides features that help us:

* Create test methods
* Run tests
* Check expected results
* Identify passed and failed tests

---

## Create the Test Project
Run:

``` bash
dotnet new xunit -n StudentApi.Tests
dotnet sln add StudentApi.Tests
dotnet add StudentApi.Tests reference StudentApi
dotnet test
```

Replace `StudentApi` with your actual project name.

### Visual Studio

### Steps

1. **Right-click the Solution** in Solution Explorer.

2. Select **Add → New Project**.

   <img width="721" height="730" alt="Add New Project" src="https://github.com/user-attachments/assets/bf9531f5-d5d7-4e7b-af1c-511987698209" />

3. In the **Create a new project** window, search for and select **xUnit Test Project**.

   <img width="1533" height="363" alt="Select xUnit Test Project" src="https://github.com/user-attachments/assets/a31b1369-9883-4426-9b47-824228531eaa" />

4. Enter the project name as **`StudentApi.Tests`** and click **Next**.

   <img width="715" height="333" alt="Name Test Project" src="https://github.com/user-attachments/assets/761a12c8-c1f5-45a3-a316-4357dea9df55" />

5. Complete the project creation steps and click **Create**.

6. Add a project reference from `StudentApi.Tests` to the main `StudentApi` project.

   - Right-click **StudentApi.Tests**.
   - Select **Add → Project Reference**.

   <img width="743" height="646" alt="Add Project Reference" src="https://github.com/user-attachments/assets/3955ff7f-bd02-42cb-bd60-b16e6e46d3a8" />

7. In the **Reference Manager**, select **StudentApi** and click **OK**.

   <img width="1408" height="751" alt="Select StudentApi Project Reference" src="https://github.com/user-attachments/assets/f2476abb-f8b4-4b35-ab8d-e617a27a2aa4" />

---

## Write Our First Test

We want to check:

> When a valid student is added, does the student count become 1?
Create this file in: StudentApi.Tests → StudentServiceTests.cs
```csharp
using Xunit;

public class StudentServiceTests
{
    [Fact]
    public void Add_ValidStudent_IncreasesCount()
    {
        // Arrange
        var service = new StudentService();

        var student = new Student
        {
            Id = 1,
            Name = "Asha",
            Marks = 85
        };

        // Act
        service.Add(student);

        // Assert
        Assert.Equal(1, service.GetCount());
    }
}
```

### What is `[Fact]`?

We just used:

```csharp
[Fact]
```

`[Fact]` tells xUnit:

> This is one test with one fixed set of data.

For example:

```text
Add student → Count should be 1
```

---

## Arrange, Act, Assert

Look at the test again:

```csharp
// Arrange
var service = new StudentService();

var student = new Student
{
    Id = 1,
    Name = "Asha",
    Marks = 85
};

// Act
service.Add(student);

// Assert
Assert.Equal(1, service.GetCount());
```

This follows **AAA**:

| Step    | Meaning               |
| ------- | --------------------- |
| Arrange | Prepare required data |
| Act     | Execute the method    |
| Assert  | Check the result      |

This pattern will be used in most unit tests.

---

## What is `Assert`?

We need a way to check whether the actual result is what we expected.

For example:

```csharp
Assert.Equal(1, service.GetCount());
```

It means:

```text
Expected → 1
Actual   → service.GetCount()
```

If both are equal, the test passes.

Some commonly used assertions are:

| Assertion            | Purpose                         |
| -------------------- | ------------------------------- |
| `Assert.Equal()`     | Checks two values are equal     |
| `Assert.NotEqual()`  | Checks two values are different |
| `Assert.Null()`      | Checks value is null            |
| `Assert.NotNull()`   | Checks value is not null        |
| `Assert.True()`      | Checks condition is true        |
| `Assert.False()`     | Checks condition is false       |
| `Assert.IsType<T>()` | Checks object type              |
| `Assert.Throws<T>()` | Checks an exception             |

We will use these when required.

---

## Run the Test

In Visual Studio:

```text
Test
 ↓
Test Explorer
 ↓
Run All
```
<img width="791" height="323" alt="image" src="https://github.com/user-attachments/assets/3bf9244c-8a55-4a9f-b79b-449078f64772" />

A successful test shows a green check mark.

If the expected result is wrong, the test fails.

---

## When We Have Multiple Inputs

Our previous test uses only one student.

What if we want to test multiple invalid marks?

For example:

```text
-5
150
```

Both should produce an exception.

Instead of writing two separate tests, xUnit provides **`[Theory]`**.

---

## `[Theory]`

`[Theory]` is used when the same test needs to run with different data.

```csharp
[Theory]
[InlineData(-5)]
[InlineData(150)]
public void Add_InvalidMarks_ThrowsException(int marks)
{
    var service = new StudentService();

    var student = new Student
    {
        Id = 2,
        Name = "Ravi",
        Marks = marks
    };

    Assert.Throws<ArgumentException>(
        () => service.Add(student)
    );
}
```

### What is `[InlineData]`?

We used:

```csharp
[InlineData(-5)]
[InlineData(150)]
```

It supplies input values to the test.

xUnit runs the test twice:

```text
marks = -5
marks = 150
```

Both values should throw `ArgumentException`.

---

## `[MemberData]`

Sometimes test data is more complex than a simple value.

Instead of writing data directly in `[InlineData]`, we can keep it separately.

```csharp
public static IEnumerable<object[]> ValidStudents =>
    new List<object[]>
    {
        new object[]
        {
            new Student
            {
                Id = 1,
                Name = "Asha",
                Marks = 85
            }
        },
        new object[]
        {
            new Student
            {
                Id = 2,
                Name = "Ravi",
                Marks = 90
            }
        }
    };
```

Use it with:

```csharp
[Theory]
[MemberData(nameof(ValidStudents))]
public void Add_ValidStudent_IncreasesCount(Student student)
{
    var service = new StudentService();

    service.Add(student);

    Assert.Equal(1, service.GetCount());
}
```

`MemberData` gets test data from a property, field, or method.

---

## `[ClassData]`

If the test data is large or needs to be reused, we can create a separate data class.
Create this file in: StudentApi.Tests → StudentTestData.cs
```csharp
public class StudentTestData : TheoryData<Student>
{
    public StudentTestData()
    {
        Add(new Student
        {
            Id = 1,
            Name = "Asha",
            Marks = 85
        });

        Add(new Student
        {
            Id = 2,
            Name = "Ravi",
            Marks = 90
        });

        Add(new Student
        {
            Id = 3,
            Name = "Neha",
            Marks = 75
        });
    }
}
```

Use the class in the test:

```csharp
[Theory]
[ClassData(typeof(StudentTestData))]
public void Add_ValidStudent_IncreasesCount(Student student)
{
    var service = new StudentService();

    service.Add(student);

    Assert.Equal(1, service.GetCount());
}
```

### Three Ways to Provide Theory Data

| Attribute      | Data Source                | Best For            |
| -------------- | -------------------------- | ------------------- |
| `[InlineData]` | Directly in attribute      | Simple values       |
| `[MemberData]` | Property, field, or method | More complex data   |
| `[ClassData]`  | Separate data class        | Reusable/large data |

---

# 3. Moq

## The Problem

Now consider a real application:

```text
Client
   ↓
Controller
   ↓
IStudentService
   ↓
Database
```

Suppose we want to test only the **Controller**.

We do not want the test to depend on the real database.

So we need a fake version of `IStudentService`.

This is where **Moq** is useful.

---

## What is Moq?

**Moq** is a mocking library for .NET.

It allows us to create a fake object for a dependency.

For example:

```text
Real Service
     ↓
  Database
```

can be replaced during testing with:

```text
Mock Service
     ↓
 Test Data
```

This allows us to test the controller independently.

---

## Install Moq

Run:

```bash
dotnet add StudentApi.Tests package Moq
```

---

## Create an Interface

Our controller will depend on this interface:
Create this file in: StudentApi → Services → IStudentService.cs
```csharp
public interface IStudentService
{
    Task<Student?> GetByIdAsync(int id);
}
```

The real service implements it:

```csharp
public class StudentService : IStudentService
{
    private readonly List<Student> _students = new()
    {
        new Student
        {
            Id = 1,
            Name = "Asha",
            Marks = 85
        }
    };

    public Task<Student?> GetByIdAsync(int id)
    {
        var student = _students.FirstOrDefault(x => x.Id == id);

        return Task.FromResult(student);
    }
}
```

---

## Controller
Create this file in: StudentApi → Controllers → StudentController.cs
```csharp
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("api/[controller]")]
public class StudentController : ControllerBase
{
    private readonly IStudentService _studentService;

    public StudentController(IStudentService studentService)
    {
        _studentService = studentService;
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetById(int id)
    {
        var student = await _studentService.GetByIdAsync(id);

        if (student == null)
            return NotFound();

        return Ok(student);
    }
}
```

Now we want to test this controller without using the real service.

---

## Create a Mock

First, create a mock:

```csharp
var mockService = new Mock<IStudentService>();
```

This creates a fake implementation of `IStudentService`.

But the mock does not automatically know what it should return.

So we configure it.

---

## `.Setup()`

Suppose we want:

```text
If GetByIdAsync(1) is called
        ↓
Return Asha
```

We use:

```csharp
mockService
    .Setup(x => x.GetByIdAsync(1))
    .ReturnsAsync(student);
```

### What happened?

```text
Setup()
   ↓
Define what the mock should do

ReturnsAsync()
   ↓
Define what it should return
```

---

## `.Object`

Our controller needs an actual `IStudentService`.

The mock itself is:

```csharp
Mock<IStudentService>
```

The mocked service object is obtained using:

```csharp
mockService.Object
```

So we can write:

```csharp
var controller =
    new StudentController(mockService.Object);
```

---

## `.Verify()`

Sometimes we also want to check:

> Was the service method actually called?

We use:

```csharp
mockService.Verify(
    x => x.GetByIdAsync(1),
    Times.Once
);
```

`Times.Once` means the method should be called exactly once.

Other useful options:

```csharp
Times.Never
Times.Once
Times.Exactly(2)
```

---

# 4. xUnit + Moq

Now we combine everything we learned.

We will test:

```text
Controller
    ↓
Mock IStudentService
    ↓
Returns test data
    ↓
Controller returns Ok()
```

## Test: Student Found
Create this file in: StudentApi.Tests → StudentControllerTests.cs
```csharp
using Microsoft.AspNetCore.Mvc;
using Moq;
using Xunit;

public class StudentControllerTests
{
    [Fact]
    public async Task GetById_ValidId_ReturnsOk()
    {
        // Arrange
        var student = new Student
        {
            Id = 1,
            Name = "Asha",
            Marks = 85
        };

        var mockService = new Mock<IStudentService>();

        mockService
            .Setup(x => x.GetByIdAsync(1))
            .ReturnsAsync(student);

        var controller =
            new StudentController(mockService.Object);

        // Act
        var result = await controller.GetById(1);

        // Assert
        var okResult =
            Assert.IsType<OkObjectResult>(result);

        var returnedStudent =
            Assert.IsType<Student>(okResult.Value);

        Assert.Equal(1, returnedStudent.Id);
        Assert.Equal("Asha", returnedStudent.Name);
        Assert.Equal(85, returnedStudent.Marks);

        // Verify
        mockService.Verify(
            x => x.GetByIdAsync(1),
            Times.Once
        );
    }
}
```

### Understand the Test Flow

```text
Arrange
   ↓
Create student
   ↓
Create mock
   ↓
Configure mock response
   ↓
Create controller

Act
   ↓
Call GetById(1)

Assert
   ↓
Check response is Ok
   ↓
Check returned student

Verify
   ↓
Check service was called once
```
<img width="862" height="345" alt="image" src="https://github.com/user-attachments/assets/37a83d50-33e9-456e-86ec-4a6163e3c5ae" />

---

## Test: Student Not Found

Now suppose the service returns `null`.

We configure the mock:

```csharp
mockService
    .Setup(x => x.GetByIdAsync(99))
    .ReturnsAsync((Student?)null);
```

Complete test:

```csharp
[Fact]
public async Task GetById_InvalidId_ReturnsNotFound()
{
    // Arrange
    var mockService = new Mock<IStudentService>();

    mockService
        .Setup(x => x.GetByIdAsync(99))
        .ReturnsAsync((Student?)null);

    var controller =
        new StudentController(mockService.Object);

    // Act
    var result = await controller.GetById(99);

    // Assert
    Assert.IsType<NotFoundResult>(result);

    // Verify
    mockService.Verify(
        x => x.GetByIdAsync(99),
        Times.Once
    );
}
```
<img width="825" height="353" alt="image" src="https://github.com/user-attachments/assets/18c0a3a4-e728-40d4-92f1-4b0021a2130e" />

---

## Test Naming

A simple naming pattern makes tests easy to understand:

```text
MethodName_Scenario_ExpectedResult
```

Examples:

```text
Add_ValidStudent_IncreasesCount

Add_InvalidMarks_ThrowsException

GetById_ValidId_ReturnsOk

GetById_InvalidId_ReturnsNotFound
```

From the name itself, we can understand what the test is checking.

---

The important idea is:

```text
xUnit → Runs and checks the test

Moq → Creates fake dependencies

xUnit + Moq → Tests a class independently
               without using real dependencies
```
