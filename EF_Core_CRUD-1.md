# Lab 10: CRUD Using EF Core – I
### Users & Roles Module

**Goal:** Implement simple CRUD (Create, Read, Update, Delete) operations for Users and Roles modules using Entity Framework Core.

---

## Table of Contents
1. [What is EF Core and Why we use it?](#1-what-is-ef-core-and-why-we-use-it)
2. [What is CRUD?](#2-what-is-crud)
3. [Understanding the Architecture](#3-understanding-the-architecture)
4. [Prerequisites](#4-prerequisites)
5. [Step 1: Role Controller](#step-1-role-controller)
6. [Step 2: User Controller](#step-2-user-controller)

---

## 1. What is EF Core and Why we use it?

**Entity Framework (EF) Core** is a lightweight, extensible, open-source, and cross-platform Object-Relational Mapper (O/RM) framework provided by Microsoft for .NET applications.

**Why we use it?**
- **No SQL Required:** It allows developers to work with a database using .NET objects, eliminating the need for most of the data-access code (SQL queries) that developers usually need to write.
- **Productivity:** It speeds up development by automating database operations (CRUD).
- **Type Safety:** We use C# classes to represent data, ensuring compile-time checking.
- **Database Agnostic:** It supports multiple databases (SQL Server, MySQL, PostgreSQL, SQLite, etc.) with minimal code changes.

---

## 2. What is CRUD?

CRUD stands for the 4 basic database operations:

| Operation | HTTP Method | SQL Equivalent | Example |
| :--- | :--- | :--- | :--- |
| **C**reate | `POST` | `INSERT INTO` | Add a new user |
| **R**ead | `GET` | `SELECT` | Get all users / Get by ID |
| **U**pdate | `PUT` | `UPDATE` | Edit user details |
| **D**elete | `DELETE` | `DELETE` | Remove a user |

In ASP.NET Core Web API, we implement CRUD using **Controllers** that handle HTTP requests and interact with the database through **Entity Framework Core (EF Core)**.

---

## 3. Understanding the Architecture

```text
Client (Swagger / Postman / Frontend)
        │
        ▼
   Controller  ──── Receives HTTP requests, validates input, returns responses
        │
        ▼
   AppDbContext ──── EF Core database context (talks to SQL Server)
        │
        ▼
     Model     ──── Entity class (represents a database table)
        │
        ▼
   Database    ──── SQL Server (actual data storage)
```

---

## 4. Prerequisites

Before starting Lab, you should already have:

-  **Models** created (`User.cs`, `Role.cs`)
-  **AppDbContext** configured with `DbSet<User>` and `DbSet<Role>`
-  **Migrations** applied
-  **NuGet Packages:** 
   - `Microsoft.EntityFrameworkCore`
   - `Microsoft.EntityFrameworkCore.SqlServer`
   - `Microsoft.EntityFrameworkCore.Tools`
   - `Microsoft.EntityFrameworkCore.Design`
   - `Scalar.AspNetCore`

**Program.cs** should be configured:

```csharp
builder.Services.AddControllers();
builder.Services.AddOpenApi();
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("connectionString")));
```

### Reference: Models & DbContext

If you haven't created them yet, here is the reference code for the Models and DbContext.

#### Role.cs
```csharp
public class Role
{
    public int RoleId { get; set; }
    public string RoleName { get; set; }

    public ICollection<User> Users { get; set; } = new List<User>(); // Navigation: A role can have many users
}
```

#### User.cs
```csharp
public class User
{
    public int UserId { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }

    public int RoleId { get; set; }      // Foreign Key
    public Role Role { get; set; }       // Navigation Property
}
```

**Relationship:**
Role (One) -> (Many) User

#### AppDbContext.cs
```csharp
using Microsoft.EntityFrameworkCore;

public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options)
        : base(options)
    {
    }

    public DbSet<User> Users => Set<User>();
    public DbSet<Role> Roles => Set<Role>();

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .HasOne(u => u.Role)
            .WithMany(r => r.Users)
            .HasForeignKey(u => u.RoleId);
    }
}
```

---

## Step 1: Role Controller

### File: Controllers/RolesController.cs

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;

[ApiController]
[Route("api/[controller]")]
public class RolesController : ControllerBase
{
    private readonly AppDbContext _context;

    public RolesController(AppDbContext context)
    {
        _context = context;
    }

    [HttpGet]
    public async Task<IActionResult> GetRoles()
    {
        var roles = await _context.Roles.ToListAsync();
        return Ok(roles);
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetRole(int id)
    {
        var role = await _context.Roles.FindAsync(id);

        if (role == null){
            return NotFound();
        }

        return Ok(role);
    }

    [HttpPost]
    public async Task<IActionResult> Create(Role role)
    {
        _context.Roles.Add(role);
        await _context.SaveChangesAsync();

        return Ok(role);
    }

    [HttpPut("{id}")]
    public async Task<IActionResult> Update(int id, Role role)
    {
        if (id != role.RoleId)
            return BadRequest();

        var oldRole = await _context.Roles.FindAsync(id);

        oldRole.RoleName = role.RoleName;
        await _context.SaveChangesAsync();

        return NoContent();
    }

    [HttpDelete("{id}")]
    public async Task<IActionResult> Delete(int id)
    {
        var role = await _context.Roles.FindAsync(id);

        if (role == null)
            return NotFound();

        _context.Roles.Remove(role);
        await _context.SaveChangesAsync();

        return Ok();
    }
}
```

---

## Step 2: User Controller

### File: Controllers/UsersController.cs

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;

[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    private readonly AppDbContext _context;

    public UsersController(AppDbContext context)
    {
        _context = context;
    }

    [HttpGet]
    public async Task<IActionResult> GetUsers()
    {
        var users = await _context.Users.ToListAsync();
        return Ok(users);
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetUser(int id)
    {
        var user = await _context.Users.FindAsync(id);

        if (user == null)
            return NotFound();

        return Ok(user);
    }

    [HttpPost]
    public async Task<IActionResult> Create(User user)
    {
        _context.Users.Add(user);
        await _context.SaveChangesAsync();

        return Ok(user);
    }

    [HttpPut("{id}")]
    public async Task<IActionResult> Update(int id, User user)
    {
        if (id != user.UserId)
            return BadRequest();
        var oldUser = await _context.Users.FindAsync(id);

        oldUser.Name = user.Name;
        oldUser.Email = user.Email;
        await _context.SaveChangesAsync();

        return Ok(user);
    }

    [HttpDelete("{id}")]
    public async Task<IActionResult> Delete(int id)
    {
        var user = await _context.Users.FindAsync(id);

        if (user == null)
            return NotFound();

        _context.Users.Remove(user);
        await _context.SaveChangesAsync();

        return Ok();
    }
}
```
