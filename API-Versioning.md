
# What is Versioning?

API versioning means updating your software without breaking the older versions that your users are already using.


<img width="1311" height="745" alt="Versioning Daigram" src="https://github.com/user-attachments/assets/7488f51f-855b-43d4-854c-8140487f80b8" />


# 1. Type of Versioning

| #   | Strategy                      | Client Sends Version Via                                                     | Example                                                | Usecase |
| --- | ----------------------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------ | ------- |
| 1   | URL Versioning                | The URL path itself                                                          | `GET /api/v1/students`                                 | Twitter API, GitHub API :- Switching Between Old & New App Version |
| 2   | Query String Versioning       | A query string parameter                                                     | `GET /api/products?api-version=1.0`                    | Amazon Product API, Flipkart API :- Applying a Filter |
| 3   | Header Versioning             | A custom request header                                                      | `GET /api/orders` + `X-Api-Version: 1.0`               | WhatsApp, Instagram API :- Auto-Checking App Version in Background |
| 4   | **Consumer-Based Versioning** | **Consumer ID; server determines the API version from the consumer mapping** | `GET /api/products` + `Consumer-Id: Client-A` → **V1** | Netflix, Banking Apps :- Recognizing Your Device/Account Automatically |

---

# 2. Install the Required NuGet Packages

Open a terminal in your project folder and run:

```bash
dotnet add package Asp.Versioning.Http
dotnet add package Asp.Versioning.Mvc.ApiExplorer
```

---

# 3. Strategy 1 — URL Versioning

The version is part of the path: `/api/v1/students`, `/api/v2/students`.

**Program.cs**

```csharp
using Asp.Versioning;
using Asp.Versioning.ApiExplorer;

builder.Services.AddApiVersioning(options =>
{
     // Set the default API version to 1.0
    options.DefaultApiVersion = new ApiVersion(1, 0);

    // Use v1.0 if no API version is provided
    options.AssumeDefaultVersionWhenUnspecified = true;

    // Add supported/deprecated API versions to response headers
    options.ReportApiVersions = true;

    // Read version from URL: /api/v1/products
    options.ApiVersionReader = new UrlSegmentApiVersionReader();

})
.AddApiExplorer(options =>
{
    // Display API groups as v1, v2, v3, etc.
    options.GroupNameFormat = "'v'VVV";

    // Replace {version} in URL with actual version number
    options.SubstituteApiVersionInUrl = true;

});
```

**`Controllers/UrlVersioning/V1/StudentsController.cs`**

```csharp
using Asp.Versioning;
using Microsoft.AspNetCore.Mvc;

namespace ApiVersioningDemo.Controllers.v1;

[ApiController]
[ApiVersion("1.0")]
[Route("api/v{version:apiVersion}/[controller]/[action]")] // Route: api/v1/users
public partial class StudentsController  : ControllerBase
{
    [HttpGet]
    public IActionResult GetUser(int id)
    {
        var userV1 = new
        {
            id = id,
            full_name = "Jane Doe",
            contact_email = "jane@example.com"
        };
        return Ok(userV1);
    }
}
```

**`Controllers/UrlVersioning/V2/StudentsController.cs`**

```csharp
using Asp.Versioning;
using Microsoft.AspNetCore.Mvc;

namespace ApiVersioningDemo.Controllers.v2;

[ApiController]
[ApiVersion("2.0")]
[Route("api/v{version:apiVersion}/[controller]/[action]")] // Route: api/v2/users
public partial class StudentsController     : ControllerBase
{
    [HttpGet]
    public IActionResult GetUser1(string id)
    {
        var userV2 = new
        {
            id = $"usr_{id}",
            name = new { first_name = "Jane", last_name = "Doe" },

        };
        return Ok(userV2);
    }
}
```

**Test:**

**URL:- https://localhost:7117/api/v2/Students/GetUser**



<img width="1767" height="952" alt="URLV1" src="https://github.com/user-attachments/assets/9944f935-3e36-43f8-a09c-5fccc9d5f83b" />


**URL:-https://localhost:7117/api/v2/Students/GetUser**

<img width="1750" height="937" alt="URLV2" src="https://github.com/user-attachments/assets/e3acf4e0-22aa-4722-abeb-c8c0f6187e5b" />



---

# 4. Strategy 2 — Query String Versioning

The version is passed as `?api-version=1.0`. The route itself has **no** version token.
**Program.cs**

```csharp
using Asp.Versioning;
using Asp.Versioning.ApiExplorer;

builder.Services.AddApiVersioning(options =>
{
    options.DefaultApiVersion = new ApiVersion(1, 0);
    options.AssumeDefaultVersionWhenUnspecified = true;
    options.ReportApiVersions = true;

    // Read version from URL: /api/v1/products
    options.ApiVersionReader = new UrlSegmentApiVersionReader();
    // Read version from Query: /api/products?api-version=1.0
    options.ApiVersionReader = new QueryStringApiVersionReader("api-version"); //New Line
})
.AddApiExplorer(options =>
{
    options.GroupNameFormat = "'v'VVV";
    options.SubstituteApiVersionInUrl = true;
});

**`Controllers/QueryVersioning/ProductsController.cs`**
using Asp.Versioning;
using Microsoft.AspNetCore.Mvc;

namespace ApiVersioningDemo.Controllers;

[ApiController]
[Route("api/[controller]")] // Static URL path: api/users
public class ProductsController : ControllerBase
{

    [HttpGet]
    [ApiVersion("1.0")]
    public IActionResult GetV1()
    {
        return Ok(new
        {
            version = "1.0",
            data = new {
                 productId = 1, name = "Keyboard", price = 799 }
        });
    }

    [HttpGet]
    [ApiVersion("2.0")]
    public IActionResult GetV2()
    {
        return Ok(new
        {
            version = "2.0",
            data = new { productId = 1, name = "Keyboard", price = 799, currency = "INR" }
        });
    }
}
```

`**Test**
**Default**
**URL:-https://localhost:7117/api/Products**


<img width="1722" height="836" alt="QueryDefault" src="https://github.com/user-attachments/assets/572e6e5a-0cdf-4f23-95d9-658aeb661873" />


**URL:-https://localhost:7117/api/Products/?api-version=1.0**


<img width="1740" height="812" alt="QueryV1" src="https://github.com/user-attachments/assets/211a84ef-836f-4f9a-9bbe-a4a3b7f62231" />


**URL:-https://localhost:7117/api/Products?api-version=2.0**


<img width="1760" height="811" alt="QueryV2" src="https://github.com/user-attachments/assets/a0719ccd-198b-428f-8528-d219b342899f" />


---

# 5. Strategy 3 — Header Versioning

Pass Version For Header Paramater `GET /api/orders` + `X-Api-Version: 1.0

**Program.cs**

```csharp
using Asp.Versioning;
using Asp.Versioning.ApiExplorer;

builder.Services.AddApiVersioning(options =>
{

    ...
    // Read version from Header: X-Version=1.0
    options.ApiVersionReader = new HeaderApiVersionReader("X-Version"); //New Line
})
.AddApiExplorer(options =>
{
    options.GroupNameFormat = "'v'VVV";
    options.SubstituteApiVersionInUrl = false; //// Header versioning does not alter URL paths
});
```

**`Controllers/HeaderVersioning/UsersController.cs`**
namespace ApiVersioningDemo.Controllers;

```csharp
[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
	[HttpGet]
	[MapToApiVersion("1.0")]
	public IActionResult GetV1()
	{
		return Ok(new { id = 101, full_name = "Jane Doe", contact_email = "jane@example.com" });
	}

	[HttpGet]
	[MapToApiVersion("2.0")]
	public IActionResult GetV2()
	{
		return Ok(new { id = "usr_101", name = new { first_name = "Jane", last_name = "Doe" }, email = "jane@example.com" });
	}
}
```

`**Test**

**GET /api/orders + `X-Api-Version: 1.0**


<img width="1731" height="817" alt="HeaderV1" src="https://github.com/user-attachments/assets/844939fa-e25e-4fbc-8745-0803bfaaa9ad" />

 
**GET /api/orders`+`X-Api-Version: 2.0**

<img width="1729" height="807" alt="HeaderV2" src="https://github.com/user-attachments/assets/3a79e967-25fa-4fd3-b9ff-6fe3b2066d5e" />

