# Lab No. 7 | Create Controllers and Action Methods and Implement CRUD APIs

## API Introduction

- API full form is ***Application Programming Interface***.
- API allows two software applications to communicate with each other.
- API defines fixed rules for sending requests and receiving responses.

## Why APIs?

- APIs separate frontend and backend work.
- APIs allow data to be shared between different applications.
- APIs make the same backend usable by web apps, mobile apps, and other clients.

## Types of APIs

- ***REST API (Representational State Transfer)*** : Uses HTTP methods like GET, POST, PUT, and DELETE.
- ***SOAP API (Simple Object Access Protocol)*** : Uses XML-based messages and strict standards.
- ***GraphQL API***: Allows clients to request only the required fields.
- ***RPC API (Remote Procedure Call)***: A protocol where a program causes a procedure to execute in another address space 
(commonly used in microservices).
- ***JSON-RPC / XML-RPC*** : Specific implementations of RPC that use JSON or XML as data formats.


## Why RESTful API?

- RESTful API is simple to create and test using browser, Scalar UI, Postman, or similar tools.
- RESTful API uses standard HTTP methods for CRUD operations.
- RESTful API returns clear HTTP status codes like 200, 201, 204, and 404.

## Project Overview

- Framework: ASP.NET Core Web API
- Storage type: Static in-memory lists inside controllers
- Database: No database is used
- Main entities:
  - `Student`
- API documentation UI:
  - Scalar UI
- Scalar URL:
  - `https://localhost:7148/scalar`
  - `http://localhost:5032/scalar`
- Packages Required:
  - Microsoft.AspNetCore.OpenApi (9.0.10)
  - Scalar.AspNetCore (2.16.5)

## To Install Required Packages

- Go to Tools > NuGet Package Manager > Manage NuGet Packages for Solution.
- Go to Browse section and write the required package name.
- Select the exact package carefully.
- Select your project.
- Install the package.

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782623599/Packages_Install_1_zwjud4.png" alt="Install Packages" style="width:100%; max-width:100%; height:auto;" />

<br>
<br>

<table>
<tr>
<td>

## Project Folder Structure

- `Controllers/StudentsController.cs`
  - Contains static Student data list.
  - Contains Student CRUD APIs.
- `Models/Student.cs`
  - Contains Student model properties.
- `Program.cs`
  - Registers controllers, OpenAPI, Scalar UI, and route mapping.

</td>
<td>

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782625710/Folder_Structure_11_zd6t2w.png" alt="Folder Structure" width="350"/>

</td>
</tr>
</table>

## Student Model

- File: `Models/Student.cs`
- Properties:
  - `Id`
  - `Name`
  - `Email`
  - `Age`

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782496348/Student_Model_2_diwkef.png" alt="Student Model" style="width:90%; max-width:90%; height:auto;" />

##### ***Note*** : We can use String.Empty or we can use '?' to show nullable property.

```csharp
    public string? Name { get; set; };
```

## Static Data Lists

- Student data list is created inside `StudentsController`.
- Data remains available while the application is running.
- Data resets when the application restarts.

## Student APIs

| Method | Endpoint | Purpose |
| --- | --- | --- |
| GET | `/api/students` | Get all students |
| GET | `/api/students/{id}` | Get one student by id |
| POST | `/api/students` | Create a new student |
| PUT | `/api/students/{id}` | Update an existing student |
| DELETE | `/api/students/{id}` | Delete a student |

## Steps To Create The Project

## Step 1: Create Models

- Create a `Models` folder.
- Create `Student.cs`.
- Add these properties:
  - `Id`
  - `Name`
  - `Email`
  - `Age`


## Step 2: Create Students Controller

- Create `StudentsController.cs` inside the `Controllers` folder.
- Add `[ApiController]` above the controller class.
- Add `[Route("api/[controller]")]` above the controller class.
- Inherit from `ControllerBase`.
- Create a static `List<Student>` inside the controller.
- Add 5 sample student records in the list.
- Add action methods for GET, POST, PUT, and DELETE.

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782496476/Student_Controller_4_zskfkz.png" alt="Student Controller" style="width:100%; max-width:100%; height:auto;" />


##### ***Note*** : We will use #region and #endregion to collapse the code in Visual Studio.

``` csharp
    #region GET api/students - Get All Student 
        // Get All Student API will be implemented here
    #endregion
```
## Step 3: Implement GET APIs For Students

- Use `[HttpGet]` for all students.
- Return `ActionResult<List<Student>>`.
- Use `[HttpGet("{id:int}")]` for a single student.
- Use `FirstOrDefault` to find a student by id.
- Return `NotFound()` if the student is missing.
- Return `Ok(student)` if the student exists.

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782496530/GET_Student_5_zatdth.png" alt="Student GET" style="width:100%; max-width:100%; height:auto;" />

## Step 4: Implement POST API For Students

- Use `[HttpPost]`.
- Accept `Student student` as a parameter.
- Generate a new id using `Max`.
- Use id `1` if the list is empty.
- Add the student using `Students.Add(student)`.
- Return `CreatedAtAction`.

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782496569/POST_Student_6_u2ajbk.png" alt="Student POST" style="width:100%; max-width:100%; height:auto;" />

##### ***Note*** : `CreatedAtAction` returns the route for the newly created student record.

## Step 5: Implement PUT API For Students

- Use `[HttpPut("{id:int}")]`.
- Accept `id` from the URL.
- Accept updated student data from the request body.
- Find the existing student using `FirstOrDefault`.
- Update `Name`, `Email`, and `Age`.
- Return `NoContent()` after successful update.

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782496609/PUT_Student_7_f7jaq7.png" alt="Student PUT" style="width:100%; max-width:100%; height:auto;" />

## Step 6: Implement DELETE API For Students

- Use `[HttpDelete("{id:int}")]`.
- Find the student by id.
- Return `NotFound()` if the student does not exist.
- Remove the student using `Students.Remove(student)`.
- Return `NoContent()`.

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782496639/DELETE_Student_8_qjd2gg.png" alt="Student DELETE" style="width:100%; max-width:100%; height:auto;" />

## Step 7: Configure Program.cs

- Add controller service:

```csharp
builder.Services.AddControllers();
```

- Add OpenAPI service:

```csharp
builder.Services.AddOpenApi();
```

- Map OpenAPI:

```csharp
app.MapOpenApi();
```

- Map Scalar UI:

```csharp
app.MapScalarApiReference();
```

- Map controllers:

```csharp
app.MapControllers();
```
<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782496716/Program_CS_10_uxjfkd.png" alt="Program.cs" style="width:100%; max-width:100%; height:auto;" />

## CRUD Operations

## Create

- HTTP method: `POST`
- Student endpoint: `/api/students`
- Used to add new records.

## Read

- HTTP method: `GET`
- Student endpoints:
  - `/api/students`
  - `/api/students/{id}`
- Used to fetch records.

## Update

- HTTP method: `PUT`
- Student endpoint: `/api/students/{id}`
- Used to modify existing records.

## Delete

- HTTP method: `DELETE`
- Student endpoint: `/api/students/{id}`
- Used to remove records.

## Important Attributes And Classes

## `[ApiController]`

- Marks the class as a Web API controller.
- Helps ASP.NET Core understand API-specific behavior.
- Helps bind request data to action method parameters.
- Helps return proper validation responses.

## `[Route]`

- Defines the base route of the controller.
- `[Route("api/[controller]")]` uses the controller name in the URL.
- `StudentsController` becomes `/api/students`.

## `[HttpGet]`

- Maps an action method to HTTP GET.
- Used to read data.
- Used for all-record and single-record APIs.

## `[HttpPost]`

- Maps an action method to HTTP POST.
- Used to create data.
- Reads new object data from the request body.

## `[HttpPut]`

- Maps an action method to HTTP PUT.
- Used to update data.
- Reads id from URL and updated object data from request body.

## `[HttpDelete]`

- Maps an action method to HTTP DELETE.
- Used to delete data.
- Reads id from URL.

## `ControllerBase`

- Base class for Web API controllers.
- Provides methods like `Ok()`, `NotFound()`, `NoContent()`, and `CreatedAtAction()`.
- Does not include MVC view features.

## `List<T>`

- Generic collection used to store multiple objects.
- `List<Student>` stores student records.
- Supports methods like `Add`, `Remove`, `Max`, and `FirstOrDefault`.

## `ActionResult`

- Represents an HTTP response from an action method.
- Can return data with status codes.
- Can return responses like `Ok()`, `NotFound()`, `NoContent()`, and `CreatedAtAction()`.

## `FirstOrDefault`

- LINQ method used to find the first matching record.
- Returns `null` if no matching record is found.
- Used to find Student or Subject by id.

## `Max`

- LINQ method used to find the highest value.
- Used to generate the next id.
- Used with empty-list check before creating a new record.

## `Ok()`

- Returns HTTP status code `200 OK`.
- Used when data is found successfully.
- Used mostly in GET APIs.

## `NotFound()`

- Returns HTTP status code `404 Not Found`.
- Used when a record does not exist.
- Used in GET by id, PUT, and DELETE APIs.

## `NoContent()`

- Returns HTTP status code `204 No Content`.
- Used after successful update.
- Used after successful delete.

## `CreatedAtAction()`

- Returns HTTP status code `201 Created`.
- Used after successful POST.
- Gives the route for the newly created record.

## How To Run The Project

- Open terminal in the project folder.
- Run:

```powershell
dotnet run
```

- Open Scalar UI:

```text
https://localhost:7148/scalar
```

- Alternative HTTP URL:

```text
http://localhost:5032/scalar
```

## Example

## Get All

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782627118/Scaller_Get_All_13_xgwkkj.png" alt="Get All API Screenshot" style="width:100%; max-width:100%; height:auto;" />

## Get By Id

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782627194/Scaller_Get_ID_15_ornjd2.png" alt="Get By Id API Screenshot" style="width:100%; max-width:100%; height:auto;" />

## Post

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782627234/Scaller_Post_14_pwjjuy.png" alt="Post API Screenshot" style="width:100%; max-width:100%; height:auto;" />

## Put

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782627270/Scaller_Put_16_ehzedj.png" alt="Put API Screenshot" style="width:100%; max-width:100%; height:auto;" />

## Delete

<img src="https://res.cloudinary.com/dbrw2yv0b/image/upload/v1782627338/Scaller_Delete_17_hx6qob.png" alt="Delete API Screenshot" style="width:100%; max-width:100%; height:auto;" />

## Tasks

```text
A   Develop a Web API for Student and Subject management using static in-memory lists.
    Implement GET endpoints to fetch all students, single student, all subjects,
    and single subject. Pre-load 5 sample student records.

B   Extend the Student & Subject Management system by implementing POST, PUT,
    and DELETE endpoints for managing student and subject records using static
    List<T> storage inside each controller.
```
