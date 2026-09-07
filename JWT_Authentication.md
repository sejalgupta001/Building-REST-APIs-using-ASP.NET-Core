# JWT Authentication in ASP.NET Core Web API

## What is JWT?

JWT is like a digital ID card. When you log in, the server gives you this card (token). You show this card on every next request so the server knows it's you.

## Use Case

Login once → get a token → use that token to access protected APIs without logging in again and again.

---

## What is the JSON Web Token Structure?

_(Ref: https://www.jwt.io/introduction)_

A JWT is just **one long string made of 3 parts, separated by dots (`.`)**:

```
xxxxx.yyyyy.zzzzz
   ↑      ↑      ↑
Header Payload Signature

```

![Token Structure](./Payload.png)

### 🔄 Which Part Changes vs Stays the Same?

| Part          | Changes?     | Why                                                                       |
| ------------- | ------------ | ------------------------------------------------------------------------- |
| **Header**    | Almost never | Same algorithm (`HS256`) every time                                       |
| **Payload**   | Every token  | New `Jti` + user data (email, role) each time                             |
| **Signature** | Every token  | It's calculated from the payload, so it changes whenever the payload does |

## Step 1: Install JWT Package

```bash
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
```

👉 This package adds the tools needed to create and check JWT tokens in your project.

---

## Step 2: Create User Model

**`Models/User.cs`**

```csharp
namespace JWTDemo.Models
{
    public class User
    {
        public int UserId { get; set; }
        public string Email { get; set; }
        public string Password { get; set; }
        public int UserTypeID { get; set; }
        public UserType UserType { get; set; }
    }
    public class UserType
    {
    public int UserTypeID { get; set; }
    public string UserTypeName { get; set; }
    }

}
```

**`Dtos/UserDTOs`**

```csharp

namespace JWTDemo.Dtos{

    public class UserLoginDto{
        public string Email { get; set; }
        public string Password { get; set; }
    }
}

```

---

## Step 3: Add JWT Settings in `appsettings.json`

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "Jwt": {
    "Key": "GQvOn3RiR/PEVdHtEv+f3z6F2x9A1z5d9h14Wg7NV9I=", // secret password to lock/unlock the token.
    "Issuer": "JwtDemoApi", // who made the token
    "Audience": "JwtDemoApiUsers", // who can use it.
    "ExpiresInMinutes": 60 // how long the token stays valid
  }
}
```

---

## Step 4: Configure JWT Authentication + Scalar

```csharp
using System.Text;
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.IdentityModel.Tokens;

builder.Services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
})
.AddJwtBearer(options =>
{
    options.TokenValidationParameters = new TokenValidationParameters
    {
        // Rules the app uses to check if a token is valid
        ValidateIssuer = true,
        ValidateAudience = true,
        ValidateLifetime = true,
        ValidateIssuerSigningKey = true, //confirms the token was signed with our secret key, not a fake one.

        ValidIssuer = builder.Configuration["Jwt:Issuer"],
        ValidAudience = builder.Configuration["Jwt:Audience"],
        IssuerSigningKey = new SymmetricSecurityKey(
            Encoding.UTF8.GetBytes(builder.Configuration["Jwt:Key"]!)
        )
    };
});

// Add the Token in Scalar Not Mandatory (if Not Added the Check in Postman)
builder.Services.AddOpenApi(options =>
{
    options.AddDocumentTransformer((document, context, cancellationToken) =>
    {
        document.Components ??= new();
        document.Components.SecuritySchemes.Add("Bearer", new()
        {
            Type = Microsoft.OpenApi.Models.SecuritySchemeType.Http,
            Scheme = "bearer",
            BearerFormat = "JWT",
            In = Microsoft.OpenApi.Models.ParameterLocation.Header,
            Description = "Enter your JWT token here (no need to type 'Bearer' prefix)"
        });
        return Task.CompletedTask;
    });
});

```

---

## Step 5: Create `TokenService.cs` & Register in `Program.cs`

**`Services/TokenService.cs`**

```csharp
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using System.Text;
using JWTDemo.Models;
using Microsoft.IdentityModel.Tokens;

namespace JWTDemo.Services
{
    public class TokenService
    {
        private readonly IConfiguration _config;

        public TokenService(IConfiguration config)
        {
            _config = config;
        }

        public string GenerateToken(User user)
        {
            //the info on the ID card (email, unique token ID)
            var claims = new[]
            {
                new Claim(JwtRegisteredClaimNames.Sub, user.Email),
                new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString())
            };
            //locks the card with our secret key
            var key = new SymmetricSecurityKey(
                Encoding.UTF8.GetBytes(_config["Jwt:Key"]!)
            );
            var credentials = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);
             //JwtSecurityToken the actual card with issuer, audience, claims, expiry
            var token = new JwtSecurityToken(
                issuer: _config["Jwt:Issuer"],
                audience: _config["Jwt:Audience"],
                claims: claims,
                expires: DateTime.UtcNow.AddMinutes(
                    double.Parse(_config["Jwt:ExpiresInMinutes"]!)),
                signingCredentials: credentials //the key + algorithm combo used to sign (seal) the token.
            );

            return new JwtSecurityTokenHandler().WriteToken(token);
        }
    }
}
```

**Register in `Program.cs`:**

```csharp
builder.Services.AddScoped<TokenService>();
```

👉 This makes `TokenService` available so `UserController` can ask for it in its constructor.

---

## Step 6: Login Endpoint — Use `TokenService` in the Controller

**`Controllers/UserController.cs`**

```csharp
using JWTDemo.Models;
using JWTDemo.Services;
using Microsoft.AspNetCore.Authorization;

namespace JWTDemo.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    [Authorize]
    public class UserController : ControllerBase
    {
        private readonly TokenService _tokenService;
        private readonly AppDbContext _context;

        public UserController(AppDbContext context,TokenService tokenService)
        {
             _context = context;
            _tokenService = tokenService;
        }
        [AllowAnonymous]
        [HttpPost("login")]
        public async Task<IActionResult> Login([FromBody] UserLoginDto dto)
        {
            try
            {
                var user = await _context.Users
                                               .SingleOrDefaultAsync(u =>
                                               u.Email == dto.Email &&
                                               u.Password == dto.Password);
                if (user == null)
                {
                    return Unauthorized("Invalid Email or password");
                }
                var token = _tokenService.GenerateToken(user);
                return Ok(new { Token = token });
            }
            catch (Exception ex)
            {
                return StatusCode(500, "Something went wrong: " + ex.Message);
            }
        }

        //Not Needed Manually to [Authorize] Keyword
        [HttpGet]
        public IActionResult GetAll()
        {
            return Ok("This is protected data!");
        }
        [AllowAnonymous] // Override controller-level [Authorize] And Make Public Method
        [HttpGet]
        public IActionResult GetAllStudentsByCategory()
        {
            return Ok("This is Public data!");
        }
    }
}
```

👉 `Login()` checks the Email/password. If correct → asks `TokenService` to make a token and sends it back. If wrong → sends `401 Unauthorized`. `[Authorize]` on `GetAll()` locks that endpoint — no valid token → `401 Unauthorized`.

---

## Step 7: Add Middleware in `Program.cs` (Order Matters)

```csharp
app.UseHttpsRedirection();
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();
```

👉 `UseAuthentication()` = "check who you are" (reads the token). `UseAuthorization()` = "check what you're allowed to do." Authentication must always come first.

---

---

## Step 9: Test the Full Flow in Scalar

**1️⃣ Generate the token — `POST /api/User/login`**

![Token Generation](./TokenGeneration.png)

**2️⃣ Call the protected endpoint with the correct token**

![Correct Token Success](./CorrectToken.png)

**3️⃣ Call the protected endpoint with a wrong/missing token**

![Wrong Token 401](./TokenIsWrong.png)

```bash
dotnet run
```

## Step 10: Add Role-Based Authentication

Only 3 Changes are Required

**1️⃣ Services/TokenService.cs**

```csharp
 var claims = new[]
            {
                new Claim(JwtRegisteredClaimNames.Sub, user.Email),
                new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString())
                new Claim(ClaimTypes.Role, user.UserType.UserTypeName) // 👈 Only new line

            };
```

**2️⃣ UserController.cs**

```csharp
var user = await _context.Users.Include(u => u.UserType) // 👈 Only new part
                               .SingleOrDefaultAsync(u =>
                               u.Email == dto.Email &&
                               u.Password == dto.Password);
```

**3️⃣ UserController.cs**

```csharp
[Authorize(Roles = "Admin")] // 👈 Only new attribute
[HttpGet]
public async Task<IActionResult> GetAllForAdmin()
{
    var user = await _context.Users.ToListAsync();
    return Ok(user);
}
```

## Step 10: Policy Based Authentication

**Role-Based = "Are you an Admin?" → Yes/No (that's it)**

**Policy-Based = "Are you an Admin AND do you meet this extra condition?"**

### 🔑 Example: Policy-Based Check (Admin Department)

| Admin User | Role    | Department  |
| ---------- | ------- | ----------- |
| Admin A    | Admin   | Account     |
| Admin C    | Admin   | Examination |
| User D     | Faculty | DIET        |

```csharp

public IActionResult AdminPageForAccount()
{
    return Ok("Visible only to Admins with Account Department");
}
```

**AdminPageForAccount() I want to Allow this Method Who have Department Account in a Admin**

Only 2 Changes
**1️⃣ Services/TokenService.cs**

```csharp
 var claims = new[]
            {
                new Claim(JwtRegisteredClaimNames.Sub, user.Email),
                new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString())
                new Claim(ClaimTypes.Role, user.UserType.UserTypeName)
                new Claim("Department", user.Department ?? "")  //Add Line
            };
```

**2️⃣ Program.cs**

```csharp
  builder.Services.AddAuthorization(options =>
 {
    // Add a custom Authorization Policy named "AdminPageForAccount"
    options.AddPolicy("AdminPageForAccount", policy =>
    {
        // 1. Check whether the logged-in user has the "Admin" role.
        //    If the user is not an Admin, access is denied (403 Forbidden).
        policy.RequireRole("Admin")

            // 2. After the Role check passes, check the "Department" claim.
            //    The Department claim must have the value "Account".
            //    If not, access is denied (403 Forbidden).
            .RequireClaim("Department", "Account");
    });
  });
```

**3️⃣ UserController.cs**

```csharp
[Authorize(Policy = "AdminPageForAccount")]
[HttpGet]
public IActionResult AdminPageForAccount()
{
    return Ok("Visible only to Admins with Account Department");
}
```
