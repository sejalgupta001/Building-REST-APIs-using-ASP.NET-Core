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

<img width="721" height="730" alt="image" src="https://github.com/user-attachments/assets/78dc9802-383e-480e-8f7e-c2101455faff" />

3. In the **Create a new project** window, search for and select **xUnit Test Project**.

<img width="1533" height="363" alt="image" src="https://github.com/user-attachments/assets/2342e52f-3e0f-4e7f-ba28-3499eb67bbc1" />

4. Enter the project name as **`StudentApi.Tests`** and click **Next**.

<img width="715" height="333" alt="image" src="https://github.com/user-attachments/assets/a3de42e4-c950-4482-b358-f7d0a7cad78a" />

5. Complete the project creation steps and click **Create**.

6. Add a project reference from `StudentApi.Tests` to the main `StudentApi` project.

   - Right-click **StudentApi.Tests**.
   - Select **Add → Project Reference**.

<img width="743" height="646" alt="image" src="https://github.com/user-attachments/assets/b99e48bc-69b1-486a-8a56-f61ad77b8a51" />

7. In the **Reference Manager**, select **StudentApi** and click **OK**.

<img width="1408" height="751" alt="image" src="https://github.com/user-attachments/assets/2a0687b2-8afa-461d-a6a2-2fdc55a72652" />

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
<img width="791" height="323" alt="image" src="https://github.com/user-attachments/assets/1d06d98c-b690-43e6-ad37-79e9f0eb6fe9" />

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
# 4. Moq with Repository and Service Layers

In a layered application:

```text
Controller
    ↓
Service
    ↓
Repository
    ↓
Database
```

For unit testing, we test **one layer at a time**:

| Class Being Tested  | Mock Dependency      |
| ------------------- | -------------------- |
| `StudentService`    | `IStudentRepository` |
| `StudentController` | `IStudentService`    |

> **Rule:** Mock the dependency of the class you are testing.

---

## 4.1 Testing Service with Mock Repository

### Repository Interface

```csharp
public interface IStudentRepository
{
    Task<Student?> GetByIdAsync(int id);
}
```

### Service

```csharp
public class StudentService : IStudentService
{
    private readonly IStudentRepository _repository;

    public StudentService(IStudentRepository repository)
    {
        _repository = repository;
    }

    public async Task<Student?> GetByIdAsync(int id)
    {
        return await _repository.GetByIdAsync(id);
    }
}
```

### Unit Test

```csharp
[Fact]
public async Task GetById_ValidId_ReturnsStudent()
{
    // Arrange

    // Create the data that we expect the repository to return
    var student = new Student
    {
        Id = 1,
        Name = "Asha",
        Marks = 85
    };

    // Create a fake repository using Moq
    // We will use this instead of the real database/repository
    var mockRepository =
        new Mock<IStudentRepository>();

    // Tell the mock:
    // "When GetByIdAsync(1) is called, return the student created above"
    mockRepository
        .Setup(x => x.GetByIdAsync(1))
        .ReturnsAsync(student);

    // Create the real StudentService
    // Pass the fake repository to the service
    var service =
        new StudentService(mockRepository.Object);


    // Act

    // Call the actual method that we want to test
    var result =
        await service.GetByIdAsync(1);


    // Assert

    // Check that a student was returned
    Assert.NotNull(result);

    // Check that the returned student's name is correct
    Assert.Equal("Asha", result.Name);


    // Verify

    // Check that the repository method was called exactly once
    mockRepository.Verify(
        x => x.GetByIdAsync(1),
        Times.Once
    );
}
```

### Flow

```text
StudentService
      ↓
Mock IStudentRepository
      ↓
Return Student
      ↓
Check Result
```

We test the **real `StudentService`**, but use a **fake repository**.

---

## 4.2 Testing Controller with Mock Service

When testing the controller, we mock `IStudentService`.

```text
StudentController
       ↓
Mock IStudentService
       ↓
Test Data
```

### Student Found

Create:

**StudentApi.Tests → StudentControllerTests.cs**

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

        var mockService =
            new Mock<IStudentService>();

        mockService
            .Setup(x => x.GetByIdAsync(1))
            .ReturnsAsync(student);

        var controller =
            new StudentController(mockService.Object);

        // Act
        var result =
            await controller.GetById(1);

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

    [Fact]
    public async Task GetById_InvalidId_ReturnsNotFound()
    {
        // Arrange
        var mockService =
            new Mock<IStudentService>();

        mockService
            .Setup(x => x.GetByIdAsync(99))
            .ReturnsAsync((Student?)null);

        var controller =
            new StudentController(mockService.Object);

        // Act
        var result =
            await controller.GetById(99);

        // Assert
        Assert.IsType<NotFoundResult>(result);

        // Verify
        mockService.Verify(
            x => x.GetByIdAsync(99),
            Times.Once
        );
    }
}
```

### Final Flow

```text
Testing Service
      ↓
Mock Repository
      ↓
Test Service Result


Testing Controller
      ↓
Mock Service
      ↓
Test Controller Result
```

> **Remember:**
> **Service Test → Mock Repository**
> **Controller Test → Mock Service**



<img width="825" height="353" alt="image" src="https://github.com/user-attachments/assets/7cc57576-32cb-4fb0-ba66-f73cb6081506" />

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
