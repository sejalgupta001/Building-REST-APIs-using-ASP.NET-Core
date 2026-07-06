# Lab No. 08 \| Working with HTTP Status Codes using In-Memory Database

## Objective

Create an **Admin Management Web API** using **ASP.NET Core Web API**.
Use an **In-Memory Database (Entity Framework Core InMemory Provider)**
and implement CRUD operations with appropriate HTTP Status Codes.

------------------------------------------------------------------------

# What is HTTP Status Code?

### HTTP status codes are messages sent by a web server to tell the client (such as a browser or an API user) whether a request was successful or not. They help identify what happened when a web page or API request is processed. HTTP status codes are divided into five categories based on the type of response.

### Status Codes Used in This Lab

## HTTP Status Codes

| Code | Name | When We Return It ? |
|------|------|-------------------|
| **1xx – Informational** | | |
| 100 | Continue | The server received the request headers and the client can continue sending the request body. |
| 101 | Switching Protocols | The server agreed to switch to another protocol. |
| 102 | Processing | The server has received the request and is processing it. |
| 103 | Early Hints | The server sends response headers before the final response. |
| **2xx – Successful** | | |
| 200 | OK | The request was successful and the requested data is returned. |
| 201 | Created | A new resource is successfully created. |
| 202 | Accepted | The request has been accepted for processing. |
| 204 | No Content | The request was successful but there is no content to return. |
| 205 | Reset Content | The client should reset the current document or form. |
| 206 | Partial Content | Only part of the requested content is returned. |
| 207 | Multi-Status | The response contains status information for multiple resources. |
| 208 | Already Reported | The resource has already been reported in the same response. |
| **3xx – Redirection** | | |
| 300 | Multiple Choices | Multiple versions of the requested resource are available. |
| 301 | Moved Permanently | The resource has permanently moved to a new URL. |
| 304 | Not Modified | The cached version can be used because the resource has not changed. |
| 306 | Switch Proxy | Reserved status code (no longer used). |
| 307 | Temporary Redirect | The resource is temporarily available at another URL. |
| 308 | Permanent Redirect | The resource has permanently moved to another URL. |
| **4xx – Client Error** | | |
| 400 | Bad Request | The request contains invalid or missing data. |
| 401 | Unauthorized | Authentication is required or credentials are invalid. |
| 403 | Forbidden | The client does not have permission to access the resource. |
| 404 | Not Found | The requested resource does not exist. |
| 405 | Method Not Allowed | The HTTP method is not supported for this resource. |
| 406 | Not Acceptable | The server cannot return the requested response format. |
| 407 | Proxy Authentication Required | Authentication with a proxy server is required. |
| 408 | Request Timeout | The server timed out waiting for the request. |
| 409 | Conflict | The request conflicts with the current state of the resource. |
| 411 | Length Required | The request must include the Content-Length header. |
| 412 | Precondition Failed | A condition in the request was not met. |
| 413 | Payload Too Large | The request body is too large for the server. |
| 414 | URI Too Long | The request URL is too long. |
| 415 | Unsupported Media Type | The server does not support the request's media type. |
| 417 | Expectation Failed | The server cannot meet the request expectations. |
| 423 | Locked | The requested resource is locked. |
| 424 | Failed Dependency | The request failed because another request failed. |
| 426 | Upgrade Required | The client must switch to a different protocol. |
| 429 | Too Many Requests | The client has sent too many requests in a short period. |
| 431 | Request Header Fields Too Large | The request headers are too large. |
| **5xx – Server Error** | | |
| 500 | Internal Server Error | An unexpected server error occurred. |
| 501 | Not Implemented | The server does not support the requested functionality. |
| 502 | Bad Gateway | The server received an invalid response from another server. |
| 503 | Service Unavailable | The server is temporarily unavailable. |
| 504 | Gateway Timeout | The server did not receive a timely response from another server. |
| 505 | HTTP Version Not Supported | The HTTP version is not supported by the server. |
| 506 | Variant Also Negotiates | The server has a configuration error. |
| 507 | Insufficient Storage | The server does not have enough storage to complete the request. |
| 508 | Loop Detected | The server detected an infinite loop while processing the request. |
| 510 | Not Extended | Additional request extensions are required. |
| 511 | Network Authentication Required | The client must authenticate to access the network. |

------------------------------------------------------------------------

# Step 1: Install Packages

``` bash
dotnet add package Microsoft.EntityFrameworkCore.InMemory
dotnet add package Scalar.AspNetCore
```

------------------------------------------------------------------------

# Step 2: Create Model

## Models/Admin.cs

``` csharp
using System.ComponentModel.DataAnnotations;

namespace Admin.Models
{
    public class Administrator
    {
        [Key]
        public int Id { get; set; }

        [Required]
        [StringLength(50, MinimumLength = 3)]
        public string Username { get; set; } = string.Empty;

        [Required]
        [StringLength(100, MinimumLength = 6)]
        public string Password { get; set; } = string.Empty;
    }
}
```

------------------------------------------------------------------------

# Step 3: Configure In-Memory Database

## Program.cs

``` csharp
using Scalar.AspNetCore;

var builder = WebApplication.CreateBuilder(args);

// Add controllers to the container
builder.Services.AddControllers();
builder.Services.AddOpenApi();



var app = builder.Build();

if (app.Environment.IsDevelopment())
{

    //Optional
    //app.MapScalarApiReference(options =>
    //{
    //    options.WithTitle("Admin Management API")
    //           .WithTheme(ScalarTheme.Purple)
    //           .WithDefaultHttpClient(ScalarTarget.CSharp, ScalarClient.HttpClient);
    //});

    {
        app.MapOpenApi();
        app.MapScalarApiReference();
    }
}

app.MapGet("/", () => Results.Redirect("/Scalar"));
app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

------------------------------------------------------------------------

# Step 4: Create Admin Controller

## Controllers/AdminController.cs

``` csharp
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using System.Linq;
using Admin.Models;

namespace Admin.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class AdminController : ControllerBase
    {
        // Declaring the static list directly inside the controller
        private static readonly List<Administrator> Admins = new List<Administrator>
        {
            new Administrator { Id = 1, Username = "admin1", Password = "Password123" },
            new Administrator { Id = 2, Username = "admin2", Password = "Password456" }
        };

        [HttpGet]
        public IActionResult GetAll()
        {
            return Ok(Admins); // 200
        }

        [HttpGet("{id}")]
        public IActionResult GetById(int id)
        {
            var admin = Admins.FirstOrDefault(x => x.Id == id);

            if (admin == null)
                return NotFound("Admin not found."); // 404

            return Ok(admin); // 200
        }

        [HttpPost]
        public IActionResult Add(Administrator admin)
        {
            if (Admins.Any(x => x.Id == admin.Id))
                return Conflict("Admin ID already exists."); // 409

            Admins.Add(admin);

            return CreatedAtAction(nameof(GetById), new { id = admin.Id }, admin); // 201
        }

        [HttpPut("{id}")]
        public IActionResult Update(int id, Administrator updatedAdmin)
        {
            var admin = Admins.FirstOrDefault(x => x.Id == id);

            if (admin == null)
                return NotFound(); // 404

            admin.Username = updatedAdmin.Username;
            admin.Password = updatedAdmin.Password;

            return NoContent(); // 204
        }

        [HttpDelete("{id}")]
        public IActionResult Delete(int id)
        {
            var admin = Admins.FirstOrDefault(x => x.Id == id);

            if (admin == null)
                return NotFound(); // 404

            Admins.Remove(admin);

            return NoContent(); // 204
        }
    }
}
```
------------------------------------------------------------------------

---
<img width="1600" height="753" alt="image1" src="https://github.com/user-attachments/assets/d95f76a3-7ff4-4610-812c-bd9b354c0cf3" />

---
<img width="1600" height="755" alt="image2" src="https://github.com/user-attachments/assets/7c720496-874a-4344-bbbf-0d533b16f136" />

---
<img width="1600" height="750" alt="image3" src="https://github.com/user-attachments/assets/bc5c8b48-8c05-4f8e-bd26-50cdc61dd958" />

---
<img width="1600" height="747" alt="image4" src="https://github.com/user-attachments/assets/d4e808e2-7d16-458c-aeec-4360d2798204" />

------------------------------------------------------------------------

# Expected Status Codes

  |API    |   Success        |   Failure                       |
  |-------|------------------|-----------------------------    |
  |GET    |   200 OK         |   404 Not Found                 |
  |POST   |   201 Created    |  400 Bad Request / 409 Conflict |
  |PUT    |   204 No Content |  404 Not Found                  |
  |DELETE |   204 No Content |  404 Not Found                  |

------------------------------------------------------------------------

## Quick Theory

An **HTTP Status Code** is a 3-digit number returned by the server to tell the client the result of its request. Status codes are grouped into five classes:

| Range | Class          | Meaning                                          |
| ----- | -------------- | ------------------------------------------------- |
| 1xx   | Informational | Request received, continuing process              |
| 2xx   | Success        | The request was completed successfully            |
| 3xx   | Redirection    | Further action is needed to complete the request  |
| 4xx   | Client Error   | The request itself has a problem                  |
| 5xx   | Server Error   | The server failed to fulfill a valid request       |

------------------------------------------------------------------------

# Lab Tasks:

Enhance the Student and Subject Management Web API by implementing appropriate HTTP status codes for all operations. Use List<Student> for in-memory storage and ensure correct status codes are returned based on operation success, failure, validation errors, and resource availability.

---
