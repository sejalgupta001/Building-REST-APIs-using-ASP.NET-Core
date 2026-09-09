# Refresh Token + Rotation in ASP.NET Core Web API

![Refresh Token](./RefreshToken.png)

## LifeTime

Access Token (JWT) = 15–60 min

Refresh Token = 7 - 30 Days

---

## What is Rotation?

**Without rotation:** one refresh token is reused again and again until it expires. If it's stolen, the thief can also keep refreshing forever.

**With rotation:** every time `/refresh` is called, the old refresh token is **thrown away** and a **new one** is saved on the user row instead.

---

## Step 1: Add Refresh Token Fields to `UserModel.cs`

**`Models/UserModel.cs`**

```csharp
using System.ComponentModel.DataAnnotations;

namespace JWTDemo.Models
{
    public class UserModel
    {
        [Key]
        public int UserId { get; set; }

        public string UserName { get; set; }

        public string Password { get; set; }

        public string Role { get; set; } = "User";

        // 👇 New fields for Refresh Token
        public string? RefreshToken { get; set; }
        public DateTime? RefreshTokenExpiryTime { get; set; }
    }
}
```

Run migration:

```bash
Add-Migration ChangesInUserModel
Update-Database
```

---

## Step 2: Add Settings in `appsettings.json`

```json
"Jwt": {
  "Key": "GQvOn3RiR/PEVdHtEv+f3z6F2x9A1z5d9h14Wg7NV9I=",
  "Issuer": "JwtDemoApi",
  "Audience": "JwtDemoApiUsers",
  "ExpiresInMinutes": 15,
  "RefreshTokenExpiresInDays": 7   // 👈 new
}
```

---

## Step 3: Update `TokenService.cs`

Add a method to generate a refresh token (just a secure random string — no need for JWT here):

```csharp
using System.Security.Cryptography;

public string GenerateRefreshToken()
{
    var randomBytes = new byte[64];
    RandomNumberGenerator.Fill(randomBytes);
    return Convert.ToBase64String(randomBytes); // long random string
}
```

---

## Step 4: Login — Issue Both Tokens & Save Refresh Token on the User Row

**`Controllers/UserController.cs`**

```csharp
[AllowAnonymous]
[HttpPost("login")]
public async Task<IActionResult> Login([FromBody] UserLoginDto dto)
{
    var user = await _context.Users
        .SingleOrDefaultAsync(u => u.UserName == dto.UserName && u.Password == dto.Password);

    if (user == null)
        return Unauthorized("Invalid Username or password");

    var accessToken = _tokenService.GenerateToken(user);
    if(user.RefreshToken== null){

    var refreshToken = _tokenService.GenerateRefreshToken(); // Add New Line

    user.RefreshToken = refreshToken; //Store In DB
    user.RefreshTokenExpiryTime = DateTime.UtcNow.AddDays(
        double.Parse(_config["Jwt:RefreshTokenExpiresInDays"]!)); // The RefreshToken ExpiryTime

    await _context.SaveChangesAsync(); // Save RefreshToken in Database
    }
    else{

        accessToken = user.RefreshToken;
    }

    return Ok(new
    {
        AccessToken = accessToken,
        RefreshToken = refreshToken
    });
}
```

---

## Step 5: The Refresh Endpoint — Where Rotation Happens


**`Controllers/UserController.cs`**

```csharp
[AllowAnonymous]
[HttpPost("refresh")]
public async Task<IActionResult> RotationRefreshToken(int userId, string refreshToken)
{
    var user = await _context.Users.SingleOrDefaultAsync(u => u.UserId == userId);

    if (user == null || user.RefreshToken != refreshToken)
        return Unauthorized("Invalid refresh token");

    if (user.RefreshTokenExpiryTime < DateTime.UtcNow)
        return Unauthorized("Refresh token expired, please log in again");


    var newAccessToken = _tokenService.GenerateToken(user);
    // Token Rotation
    var newRefreshToken = _tokenService.GenerateRefreshToken();
    user.RefreshToken = newRefreshToken;
    user.RefreshTokenExpiryTime = DateTime.UtcNow.AddDays(
        double.Parse(_config["Jwt:RefreshTokenExpiresInDays"]!));

    await _context.SaveChangesAsync();

    return Ok(new
    {
        AccessToken = newAccessToken,
        RefreshToken = newRefreshToken
    });
}
```

---

## Step 6: Logout — Clear the Refresh Token

```csharp
[HttpPost("logout")]
public async Task<IActionResult> Logout([FromBody] int userId)
{
   var user = await _context.Users.SingleOrDefaultAsync(u => u.UserId == userId);
    if (user == null) return NotFound();

    return Ok("Logged out");
}
```

---
