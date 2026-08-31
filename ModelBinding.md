# Model Binding in ASP.NET Core Web API

Model Binding in ASP.NET Core is the process of automatically reading data from an HTTP request and passing it directly to your controller action or model object.

Instead of writing code to manually pull values out of the request, ASP.NET Core does it for you automatically.
For example, if a request sends JSON data, a query string, or a route value, ASP.NET Core reads it and fills in your method parameters — no extra work needed.

## Advantages of Model Binding

- **Saves Developer Time:** You don't have to write code to read and parse request data manually.
- **Cleaner Code:** Your controller actions stay simple, readable, and focused on business logic.
- **Auto Type Conversion:** It automatically converts text from HTTP requests into the right .NET types (like string to int).
- **Works Everywhere:** It can read data from many parts of an HTTP request using simple attributes.

## When to Use Model Binding
Use model binding when your API needs to receive data from:

- The request body (`[FromBody]`)
- The URL query string (`[FromQuery]`)
- The URL route/path (`[FromRoute]`)
- HTTP headers (`[FromHeader]`)
- HTML form data or file uploads (`[FromForm]`)

## Binding Sources Explained

### 1. `[FromBody]`

Use `[FromBody]` when the client sends data inside the request body — usually as JSON.
It is used by default if not used Explictly.

**Example:**

```csharp
[HttpPost("api/User")]
public IActionResult CreateUser([FromBody] UserCreateDto userCreateDto)
{
    var user = new User
    {
        Name = userCreateDto.Name,
        Email = userCreateDto.Email,
        PasswordHash = BCrypt.Net.BCrypt.HashPassword(userCreateDto.PasswordHash),
        Role = userCreateDto.Role,
        DepartmentId = userCreateDto.DepartmentId
    };

    context.Users.Add(user);
    context.SaveChanges();

    return Ok(ToUserResponseDto(user));
}
```
![query parameter image 1](https://res.cloudinary.com/dt2ohlevm/image/upload/v1786013206/1_jtgsr2.png)

Use this when you're sending a full object (like a form or product details) as JSON in the request body.

### 2. `[FromQuery]`

Use `[FromQuery]` when values are passed in the URL after the `?` symbol.
This is great for filtering, searching, sorting, and pagination.

**Example:**
1.you are doing pagination you will send pageNumber and pageSize as query parameters.

```csharp
[HttpGet("api/projects")]
public async Task<IActionResult> GetAllTasks([FromQuery]int pageNumber = 1,[FromQuery]int pageSize = 10)
{
    if (pageNumber < 1 || pageSize < 1) return BadRequest(...);

        var query = _db.Tasks.AsNoTracking().Where(t => !t.IsDeleted).AsQueryable();
        var totalCount = await query.CountAsync();

        //pagination logic here

        return Ok(items);
}
```
The Req URl Will be:
http://localhost:5000/api/Task?Status=&PageNumber=1&PageSize=10

![query parameter image 2](https://res.cloudinary.com/dt2ohlevm/image/upload/v1786013206/2_ti9r5b.png)

**Example:**
2.you can pass an object as well in `[FormQuery]` to do advance pagination,filtering, searching, sorting as well.
```csharp
[HttpGet("api/projects")]
public async Task<IActionResult> GetAllTasks([FromQuery] TaskQueryParameters parameters)
{
    if (pageNumber < 1 || pageSize < 1) return BadRequest(...);

        var query = _db.Tasks.AsNoTracking().Where(t => !t.IsDeleted).AsQueryable();
        var totalCount = await query.CountAsync();

        //all the logic here

        return Ok(items);
}
```
The Req URl Will be:
http://localhost:5000/api/Task?Status=pending&Priority=low&PageNumber=1&PageSize=10

![query parameter image 3](https://res.cloudinary.com/dt2ohlevm/image/upload/v1786013206/3_no4caz.png)

### 3. `[FromRoute]`

Use `[FromRoute]` when a value is part of the URL path itself (defined in the route template).

Use this when you need to identify a specific resource by its ID in the URL.
**Example:**

```csharp
[HttpGet("api/Task/{Id}")]
public IActionResult GetTaskById([FromRoute] long id)
{
    var task = context.Tasks.FirstOrDefault(item => item.Id == id);

    if (task == null)
    {
        return NotFound();
    }

    return Ok(ToResponseTaskDto(task));
}
```
![query parameter image 1](https://res.cloudinary.com/dt2ohlevm/image/upload/v1786013206/5_dchcmc.png)

### 4. `[FromForm]`

Use `[FromForm]` when the client submits an HTML form or uploads a file using `multipart/form-data`.

This is the right choice for file uploads and traditional form submissions.
**Example:**

```csharp
[HttpPost("api/users/upload")]
public async Task<IActionResult> UploadProfile([FromForm] UserProfileForm form)
{
    // Handles form data and file uploads
    await Task.CompletedTask;
    return Ok();
}
```

![query parameter image 1](https://res.cloudinary.com/dt2ohlevm/image/upload/v1786013206/4_rtvd2z.png)

### 5. `[FromHeader]`

Use `[FromHeader]` when the value is sent as an HTTP header (not in the URL or body).

**Example:**

```csharp
[HttpGet("api/projects")]
public async Task<IActionResult> GetProjects([FromHeader(Name = "X-User-Id")] string userId)
{
    // userId is read from the request header: X-User-Id: 101
    await Task.CompletedTask;
    return Ok();
}
```
<img width="1390" height="606" alt="image" src="https://github.com/user-attachments/assets/21d9cbc6-f9bf-4c8c-ad60-c680fe492a4a" />


This is useful for passing tokens, user IDs, or other metadata through headers.


## Which One Should You Use?

- Use `[FromBody]` when sending JSON or a complex object in the request body.
- Use `[FromQuery]` for search filters, page numbers, or optional URL parameters.
- Use `[FromRoute]` for IDs or values that are part of the URL path.
- Use `[FromHeader]` for tokens, metadata, or custom header values.
- Use `[FromForm]` for file uploads or HTML form submissions.

## Best Practices

- Keep your parameter names short and your routes easy to read.
- Always validate your models using Data Annotations or FluentValidation.
- Add the `[ApiController]` attribute to your controller — it automatically handles `ModelState` validation and returns proper error responses.
- Keep your action methods focused on business logic. Let model binding handle all the data reading.

## Conclusion

Model binding acts as a bridge between raw HTTP request data and your .NET method parameters.

It removes the need to write repetitive parsing code, keeping your controllers clean and easy to maintain.

Choosing the right binding attribute — `[FromBody]`, `[FromQuery]`, `[FromRoute]`, `[FromHeader]`, or `[FromForm]` — is key to building clean and reliable ASP.NET Core Web APIs.


| Attribute      | Data Source                     | Common Usage                      | Example Request                 | Supports Complex Objects | Common Error                          | Best Use Case                            |
| -------------- | ------------------------------- | --------------------------------- | ------------------------------- | ------------------------ | ------------------------------------- | ---------------------------------------- |
| `[FromBody]`   | HTTP Request Body               | Create / Update operations        | POST JSON payload               | Yes                      | Invalid JSON, missing required fields | Receive JSON/XML or large complex data   |
| `[FromQuery]`  | URL Query String                | Filtering, sorting, pagination    | `/api/projects?page=1&sort=asc` | Limited                  | Missing query parameters              | Search, filter, optional parameters      |
| `[FromRoute]`  | URL Route Parameters            | Resource identification           | `/api/projects/5`               | No                       | Route parameter missing               | Unique entity ID in URL                  |
| `[FromHeader]` | HTTP Headers                    | Tokens, metadata, correlation IDs | `X-User-Id: 101`                | No                       | Header not found                      | Authentication tokens / request metadata |
| `[FromForm]`   | Form data / multipart form-data | File uploads, form submissions    | Multipart/form-data request     | Yes                      | Incorrect Content-Type                | File upload and form submission          |
