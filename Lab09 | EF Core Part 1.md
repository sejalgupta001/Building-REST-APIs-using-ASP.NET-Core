# LAB 9

## What We're Building

A **Student Project Management System** where:

- Users (students, professors, admins) sign in and work on projects
- Roles & Permissions control what each user can do

In this lab we create the **core identity models** — Users, Roles, and Permissions — and set up EF Core from scratch.

---

## 1. What Is an ORM?

An **ORM** is a tool that connects **objects in code** with **tables in a database**.

In simple terms:

- a C# class becomes a database table
- a C# object becomes a table row
- a property becomes a column

EF Core is Microsoft's ORM for C#. It lets you work with the database using C# instead of writing SQL for every action.

---

## 2. EF Core Basics

These are the main EF Core ideas you will use in this course:

| Term | Meaning |
| --- | --- |
| Entity | A C# class that represents a table |
| DbContext | The main class that connects your app to the database |
| DbSet&lt;T&gt; | A collection that represents one table |
| Migration | A version of your database schema |
| LINQ | The C# way to query data |

**This course uses Code-First** — you write models first, then EF Core creates the database.

| Database Concept | C# Equivalent |
| :--- | ---: |
| Table | Class |
| Row | Object (instance) |
| Column | Property |
| Primary Key | `Id` property |
| Foreign Key | Property pointing to another class |

---

## 3. Code-First vs Database-First (First Look)

Two ways to build the database:

| Approach | What you do first | Best for |
| :--- | ---: | ---: |
| **Code-First** | Write C# models → EF Core generates the database | New projects (you own the schema) |
| **Database-First** | Design database first → EF Core generates C# models from existing tables | Working with an existing/shared database |

**This course uses Code-First** — you write models, EF Core creates the database.

---

## 4. Create the Project

1. Open a terminal in the folder where you want the project.
2. Create the Web API project.
3. Move into the project folder.

```bash
dotnet new webapi -n StudentProjectAPI
cd StudentProjectAPI
```

---

## 5. Install EF Core Packages

1. Add the core EF Core package.
2. Add the design package for migrations.
3. Add the SQL Server provider.
4. Add the tools package for CLI commands.

> Warning: if your starter project already includes an OpenAPI-related package, keep it pinned to the course-approved version `2.7.5`. Do not upgrade it to a newer version in this lab if NuGet reports vulnerability warnings.

```bash
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

| Package | Why |
| --- | --- |
| `EntityFrameworkCore` | Core ORM engine |
| `.Design` | Needed for migrations |
| `.SqlServer` | Talks to SQL Server |
| `.Tools` | Enables migration CLI commands |

---

## 6. Design the Tables: Users, Roles, Permissions

### Role

| Column | Type | Constraints |
| --- | --- | --- |
| Id | int | PK, Identity |
| Name | nvarchar(50) | Not Null, Unique |
| Description | nvarchar(200) | Null |

### Permission

| Column | Type | Constraints |
| --- | --- | --- |
| Id | int | PK, Identity |
| Name | nvarchar(100) | Not Null, Unique |

### User

| Column | Type | Constraints |
| --- | --- | --- |
| Id | int | PK, Identity |
| FullName | nvarchar(100) | Not Null |
| Email | nvarchar(150) | Not Null, Unique |
| Password | nvarchar(255) | Not Null |
| RoleId | int | FK → Role(Id), Not Null |

### RolePermission (Junction Table)

| Column | Type | Constraints |
| --- | --- | --- |
| RoleId | int | FK → Role(Id) |
| PermissionId | int | FK → Permission(Id) |

Composite PK on (RoleId, PermissionId) — this is a **many-to-many** relationship.

### Relationship Summary

| Relationship | Type |
| --- | --- |
| Role → User | One-to-Many (one role has many users) |
| Role ↔ Permission | Many-to-Many (a role has many permissions, a permission belongs to many roles) |

---

## 7. Create the Models (C# Classes)

Create a `Models/` folder and add these files.

### Models/Role.cs

```csharp
using System.ComponentModel.DataAnnotations;

namespace StudentProjectAPI.Models;

public class Role
{
    [Key]
    public int Id { get; set; }

    [Required, MaxLength(50)]
    public string Name { get; set; } = string.Empty;

    [MaxLength(200)]
    public string? Description { get; set; }

    // Navigation: A role can have many users
    public ICollection<User> Users { get; set; } = new List<User>();

    // Navigation: A role has many permissions (many-to-many)
    public ICollection<RolePermission> RolePermissions { get; set; } = new List<RolePermission>();
}
```

### Models/Permission.cs

```csharp
using System.ComponentModel.DataAnnotations;

namespace StudentProjectAPI.Models;

public class Permission
{
    [Key]
    public int Id { get; set; }

    [Required, MaxLength(100)]
    public string Name { get; set; } = string.Empty;

    // Navigation: A permission belongs to many roles
    public ICollection<RolePermission> RolePermissions { get; set; } = new List<RolePermission>();
}
```

### Models/RolePermission.cs (Junction Table)

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace StudentProjectAPI.Models;

public class RolePermission
{
    [Key]
    public int Id { get; set; }

    [ForeignKey("Role")]
    public int RoleId { get; set; }
    public Role? Role { get; set; }

    [ForeignKey("Permission")]
    public int PermissionId { get; set; }
    public Permission? Permission { get; set; }
}
```

### Models/User.cs

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace StudentProjectAPI.Models;

public class User
{
    [Key]
    public int Id { get; set; }

    [Required, MaxLength(100)]
    public string FullName { get; set; } = string.Empty;

    [Required, MaxLength(150)]
    public string Email { get; set; } = string.Empty;

    [Required, MaxLength(255)]
    public string Password { get; set; } = string.Empty;

    // Foreign Key → Role
    [ForeignKey("Role")]
    public int RoleId { get; set; }

    // Navigation: A user belongs to one role
    public Role? Role { get; set; }
}
```

### Navigation Properties, in Simple Terms

Classes by themselves are just plain data holders. **Navigation properties** tell EF Core how one table is connected to another.

Think of them like links between objects.

| Property | What it does |
| --- | --- |
| `public Role? Role { get; set; }` | Lets a User point to its Role |
| `public ICollection<User> Users { get; set; }` | Lets a Role keep a list of Users |
| `public ICollection<RolePermission> RolePermissions { get; set; }` | Connects Roles and Permissions |

Without navigation properties, EF Core cannot figure out relationships. They are the **"objects" that make relational data work in C#**.

---

## 8. Create the DbContext

Create `Data/AppDbContext.cs`:

This file is usually created **manually** in a code-first project. EF Core does not generate it for you here.

In simple words, `DbContext` is the main class that tells EF Core which tables to work with.

### What to add inside it

- a constructor that accepts `DbContextOptions<AppDbContext>`
- one `DbSet<T>` for each table
- `OnModelCreating()` for small rules like unique indexes

In this lab, we keep it short and basic. In **Lab 10**, we explain `DbContext` in more detail.

```csharp
using Microsoft.EntityFrameworkCore;
using StudentProjectAPI.Models;

namespace StudentProjectAPI.Data;

public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options)
        : base(options) { }

    public DbSet<User> Users => Set<User>();
    public DbSet<Role> Roles => Set<Role>();
    public DbSet<Permission> Permissions => Set<Permission>();
    public DbSet<RolePermission> RolePermissions => Set<RolePermission>();

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // Unique constraints
        modelBuilder.Entity<User>()
            .HasIndex(u => u.Email)
            .IsUnique();

        modelBuilder.Entity<Role>()
            .HasIndex(r => r.Name)
            .IsUnique();

        modelBuilder.Entity<Permission>()
            .HasIndex(p => p.Name)
            .IsUnique();

        // Prevent duplicate role-permission pairs
        modelBuilder.Entity<RolePermission>()
            .HasIndex(rp => new { rp.RoleId, rp.PermissionId })
            .IsUnique();
    }
}
```

### DbSet vs DbContext

| DbContext | DbSet&lt;T&gt; |
| --- | --- |
| Represents the **whole database** | Represents **one table** |
| Contains multiple `DbSet` properties | Collection of entity `T` |
| Manages connections, transactions, saving | Used for LINQ queries & CRUD |

---

## 9. Add Connection String

In `appsettings.json`:

```json
{
  "ConnectionStrings": {
    "Default": "Server=.;Database=StudentProjectDb;Trusted_Connection=True;TrustServerCertificate=True;MultipleActiveResultSets=true"
  }
}
```

If you are using a different SQL Server setup, update the value to match your machine.

Common fixes:

- Use `Server=.;` for a local SQL Server instance.
- Use `Server=(localdb)\MSSQLLocalDB;` if you are using LocalDB.
- Keep `TrustServerCertificate=True` when SQL Server uses a self-signed certificate.
- If you see a login or network error, verify the server name, database name, and authentication mode.

If you want to keep the connection string in `Program.cs`, this is the same value you will pass into `UseSqlServer()`.

---

## 10. Register DbContext in Program.cs

Add the OpenAPI/Scalar packages first (not included by default):

```bash
dotnet add package Microsoft.AspNetCore.OpenApi
dotnet add package Scalar.AspNetCore
```

If your project already references an OpenAPI package version, pin it to `2.7.5` in the `.csproj` file instead of floating to a newer version.

### Disable Warning Failures in the Project File

Sometimes `Add-Migration` in the NuGet Package Manager Console stops because restore warnings are treated as errors. Add the following to your project file if that happens:

```xml
<PropertyGroup>
    <NoWarn>$(NoWarn);NU1901;NU1902;NU1903;NU1904</NoWarn>
    <WarningsNotAsErrors>$(WarningsNotAsErrors);NU1901;NU1902;NU1903;NU1904</WarningsNotAsErrors>
</PropertyGroup>
```

If your project also has `TreatWarningsAsErrors` enabled, keep it off for the warnings above so migrations can run successfully.

```csharp
using Scalar.AspNetCore;
using Microsoft.EntityFrameworkCore;
using StudentProjectAPI.Data;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddOpenApi();

builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("Default")));

var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
    app.MapScalarApiReference();
}

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();
app.Run();
```

---

## 11. Create & Apply the First Migration

### Step 0: Install the EF Core CLI tool (one-time, if not already installed)

```bash
dotnet tool install --global dotnet-ef
```

If `Add-Migration` still fails in Package Manager Console, check whether the project file is turning NuGet warnings into build failures and apply the warning suppression shown above.

### Step 1: Create the migration

```bash
dotnet ef migrations add InitialCreate
```

This generates a `Migrations/` folder with two files:

- `XXXXXXXXXXXXXX_InitialCreate.cs` — contains `Up()` and `Down()` methods
- `AppDbContextModelSnapshot.cs` — a snapshot of your current model

### Step 2: Inspect the generated code

```csharp
// Inside the migration file
public partial class InitialCreate : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        // Creates all tables with columns, PKs, FKs, indexes
        migrationBuilder.CreateTable(name: "Roles", ...);
        migrationBuilder.CreateTable(name: "Permissions", ...);
        migrationBuilder.CreateTable(name: "Users", ...);
        migrationBuilder.CreateTable(name: "RolePermissions", ...);
    }

    protected override void Down(MigrationBuilder migrationBuilder)
    {
        // Reverses everything (drops tables in reverse order)
        migrationBuilder.DropTable(name: "RolePermissions");
        migrationBuilder.DropTable(name: "Users");
        migrationBuilder.DropTable(name: "Permissions");
        migrationBuilder.DropTable(name: "Roles");
    }
}
```

### Step 3: Apply to database

```bash
dotnet ef database update
```

This runs `Up()` against your database, creating real SQL tables.

---

## 12. How Migrations Work

| Concept | What it means |
| --- | --- |
| **Migration** | A version of your database schema |
| `Up()` | Forward changes — "add this table, add that column" |
| `Down()` | Rollback — "undo what Up() did" |
| **Snapshot** | EF Core's internal model of what your schema should look like |
| **Why migrations?** | Schema stays in version control alongside code. Team members sync by running `database update`. |

### Workflow

```text
Change models → dotnet ef migrations add Name → Review Up()/Down() → dotnet ef database update
```

### Key Commands

| Command | What it does |
| --- | --- |
| `dotnet ef migrations add Name` | Create a new migration |
| `dotnet ef database update` | Apply pending migrations |
| `dotnet ef migrations list` | Show all migrations |
| `dotnet ef migrations remove` | Remove the last migration (if not applied) |

---

## 13. Common Mistakes (Students)

| Problem | Fix |
| --- | --- |
| `No DbContext found` | Install `Microsoft.EntityFrameworkCore.Design` |
| `The entity type requires a primary key` | Add `[Key]` or name it `Id` |
| `Foreign key constraint error` | Check FK properties match exactly |
| `Cannot insert duplicate key` | Add `[Index(IsUnique = true)]` for unique columns |

---

## What's Next?

In **Lab 10** we add **Projects, Tasks, and ProjectAllocation** models with more advanced relationships, and learn Code-First vs Database-First in detail.
