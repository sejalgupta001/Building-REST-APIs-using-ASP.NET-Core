# LAB 10

## Recap

In Lab 9 we built the core identity models (User, Role, Permission) and set up EF Core.

Now we add the **project domain**: Projects, Tasks, and who is allocated to what.

---

## 1. Code-First vs Database-First — Explained

### Code-First (What We Use)

You write C# classes → EF Core generates the database.

**Flow:**

```text
Write C# models → Add migration → Update database
```

**Pros:**

- Full control in code — schema changes alongside your app
- Easy to version control (migration files go to Git)
- No SQL needed for schema creation
- Fast prototyping — change a model, create a migration, done

**Cons:**

- Need to understand how EF Core translates C# to SQL
- Generated SQL may not be perfectly optimized
- DBA team may prefer owning the schema

### Database-First

You design the database first → EF Core generates C# models from tables.

**Flow:**

```text
Design database → Scaffold-DbContext → Generated models + context
```

**Command:**

```bash
dotnet ef dbcontext scaffold "YourConnectionString" Microsoft.EntityFrameworkCore.SqlServer
```

**Pros:**

- Great for existing databases you don't own
- DBA designs schema, devs consume it
- No migrations to manage

**Cons:**

- Rerun scaffold every time the database changes
- Generated code can be messy
- Lose the migration history / versioning

### When to Use Which

| Scenario | Approach |
| --- | --- |
| New project, you own the DB | **Code-First** ✅ |
| Existing database shared across teams | **Database-First** |
| You want schema in Git | **Code-First** |
| DBA controls all schema changes | **Database-First** |
| Quick prototype → production app | Code-First (fast) → Either |

---

## 2. Design the Tables: Projects, Tasks, Allocations

### Project

| Column | Type | Constraints |
| --- | --- | --- |
| Id | int | PK, Identity |
| Title | nvarchar(200) | Not Null |
| Description | nvarchar(2000) | Null |
| StartDate | datetime2 | Not Null |
| EndDate | datetime2 | Null |
| Status | nvarchar(20) | Not Null (NotStarted / InProgress / Completed) |
| CreatedByUserId | int | FK → User(Id), Not Null |

### ProjectTask

| Column | Type | Constraints |
| --- | --- | --- |
| Id | int | PK, Identity |
| Title | nvarchar(200) | Not Null |
| Description | nvarchar(2000) | Null |
| Deadline | datetime2 | Null |
| Status | nvarchar(20) | Not Null (Pending / InProgress / Completed) |
| ProjectId | int | FK → Project(Id), Not Null |
| AssignedToUserId | int | FK → User(Id), Null |

### ProjectAllocation

| Column | Type | Constraints |
| --- | --- | --- |
| Id | int | PK, Identity |
| ProjectId | int | FK → Project(Id), Not Null |
| UserId | int | FK → User(Id), Not Null |
| Role | nvarchar(50) | Not Null (e.g., "Developer", "Reviewer", "TL") |

### Relationship Summary

| Relationship | Type |
| --- | --- |
| User → Project (created by) | One-to-Many |
| User → ProjectAllocation | One-to-Many |
| Project → ProjectTask | One-to-Many |
| Project → ProjectAllocation | One-to-Many |
| User → ProjectTask (assigned to) | One-to-Many |

---

## 3. Add the New Models

We already have `Models/` from Lab 9. Add these files.

### Models/Project.cs

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace StudentProjectAPI.Models;

public class Project
{
    [Key]
    public int Id { get; set; }

    [Required, MaxLength(200)]
    public string Title { get; set; } = string.Empty;

    [MaxLength(2000)]
    public string? Description { get; set; }

    [Required]
    public DateTime StartDate { get; set; }

    public DateTime? EndDate { get; set; }

    [Required, MaxLength(20)]
    public string Status { get; set; } = "NotStarted";

    // FK → User (who created this project)
    [ForeignKey("CreatedByUser")]
    public int CreatedByUserId { get; set; }
    public User? CreatedByUser { get; set; }

    // Navigation: A project has many tasks
    public ICollection<ProjectTask> Tasks { get; set; } = new List<ProjectTask>();

    // Navigation: A project has many allocations
    public ICollection<ProjectAllocation> ProjectAllocations { get; set; } = new List<ProjectAllocation>();
}
```

### Models/ProjectTask.cs

> We name the class `ProjectTask`, not `Task` — `Task` clashes with `System.Threading.Tasks.Task`, which every `async` method uses. `[Table("Tasks")]` keeps the actual DB table named `Tasks`.

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace StudentProjectAPI.Models;

[Table("Tasks")]
public class ProjectTask
{
    [Key]
    public int Id { get; set; }

    [Required, MaxLength(200)]
    public string Title { get; set; } = string.Empty;

    [MaxLength(2000)]
    public string? Description { get; set; }

    public DateTime? Deadline { get; set; }

    [Required, MaxLength(20)]
    public string Status { get; set; } = "Pending";

    // FK → Project
    [ForeignKey("Project")]
    public int ProjectId { get; set; }
    public Project? Project { get; set; }

    // FK → User (who is assigned)
    [ForeignKey("AssignedToUser")]
    public int? AssignedToUserId { get; set; }
    public User? AssignedToUser { get; set; }
}
```

### Models/ProjectAllocation.cs

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace StudentProjectAPI.Models;

public class ProjectAllocation
{
    [Key]
    public int Id { get; set; }

    [ForeignKey("Project")]
    public int ProjectId { get; set; }
    public Project? Project { get; set; }

    [ForeignKey("User")]
    public int UserId { get; set; }
    public User? User { get; set; }

    [Required, MaxLength(50)]
    public string Role { get; set; } = string.Empty; // Developer, Reviewer, TL, etc.
}
```

---

## 4. Update the DbContext

Add the new `DbSet` properties to `Data/AppDbContext.cs`:

This is the same `DbContext` file you created in Lab 9. In a code-first project, you usually write this file yourself, and then keep updating it as you add new tables.

In simple words, `DbContext` is the class that tells EF Core which entities belong to the database and how they connect to each other.

### How to update it

1. Open `Data/AppDbContext.cs`.
2. Add one `DbSet<T>` for every new model.
3. Add extra rules inside `OnModelCreating()` when needed.
4. Save the file and create a new migration.

### What belongs here?

- `DbSet<T>` properties for tables like `Projects`, `Tasks`, and `ProjectAllocations`
- relationship rules
- unique indexes or composite keys

For this lab, we focus on the new project tables and keep the explanation practical.

```csharp
public DbSet<Project> Projects => Set<Project>();
public DbSet<ProjectTask> Tasks => Set<ProjectTask>();
public DbSet<ProjectAllocation> ProjectAllocations => Set<ProjectAllocation>();
```

Add any extra configuration in `OnModelCreating`:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    // ... existing config from Lab 9 ...

    // Unique index on allocation (prevent duplicate assignments)
    modelBuilder.Entity<ProjectAllocation>()
        .HasIndex(pa => new { pa.ProjectId, pa.UserId })
        .IsUnique();
}
```

---

## 5. Create the Migration

```bash
dotnet ef migrations add AddProjectsTasksAllocations
```

### Examine the Generated Code

Open the migration file. You'll see `Up()` creates three tables:

```csharp
protected override void Up(MigrationBuilder migrationBuilder)
{
    migrationBuilder.CreateTable(
        name: "Projects",
        columns: table => new
        {
            Id = table.Column<int>(type: "int", nullable: false)
                .Annotation("SqlServer:Identity", "1, 1"),
            Title = table.Column<string>(type: "nvarchar(200)", maxLength: 200, nullable: false),
            Description = table.Column<string>(type: "nvarchar(2000)", maxLength: 2000, nullable: true),
            StartDate = table.Column<DateTime>(type: "datetime2", nullable: false),
            EndDate = table.Column<DateTime>(type: "datetime2", nullable: true),
            Status = table.Column<string>(type: "nvarchar(20)", maxLength: 20, nullable: false),
            CreatedByUserId = table.Column<int>(type: "int", nullable: false)
        },
        constraints: table =>
        {
            table.PrimaryKey("PK_Projects", x => x.Id);
            table.ForeignKey(
                name: "FK_Projects_Users_CreatedByUserId",
                column: x => x.CreatedByUserId,
                principalTable: "Users",
                principalColumn: "Id",
                onDelete: ReferentialAction.Cascade);
        });

    // Similar CreateTable calls for Tasks and ProjectAllocations
}

protected override void Down(MigrationBuilder migrationBuilder)
{
    migrationBuilder.DropTable(name: "ProjectAllocations");
    migrationBuilder.DropTable(name: "Tasks");
    migrationBuilder.DropTable(name: "Projects");
}
```

### Apply

```bash
dotnet ef database update
```

---

## 6. Understanding Migration History

After two migrations, you have:

```text
Migrations/
├── 20250101000100_InitialCreate.cs            # Lab 9
├── 20250101000200_AddProjectsTasksAllocations.cs  # Lab 9.2
└── AppDbContextModelSnapshot.cs
```

The **Snapshot** file stores the combined model of ALL migrations so far. EF Core compares the snapshot with your current C# models to detect what changed.

### Need to undo?

```bash
# Roll back to before the new migration
dotnet ef database update InitialCreate

# Then remove the migration file
dotnet ef migrations remove
```

---

## 7. How to Add a Column Later

Add a property to any model:

```csharp
// In Project.cs
[MaxLength(500)]
public string? ClientName { get; set; }
```

Then:

```bash
dotnet ef migrations add AddClientNameToProject
dotnet ef database update
```

The generated `Up()` will contain:

```csharp
migrationBuilder.AddColumn<string>(
    name: "ClientName",
    table: "Projects",
    type: "nvarchar(500)",
    maxLength: 500,
    nullable: true);
```

---

## 8. Full Entity Diagram (Both Labs)

```text
┌─────────────┐     ┌──────────────────┐     ┌──────────────┐
│    Role     │     │  RolePermission  │     │  Permission  │
├─────────────┤     ├──────────────────┤     ├──────────────┤
│ Id (PK)     │────→│ RoleId (FK)      │←────│ Id (PK)      │
│ Name        │     │ PermissionId (FK)│     │ Name         │
│ Description │     └──────────────────┘     └──────────────┘
└──────┬──────┘
       │
       ▼
┌──────────────┐     ┌──────────────────┐
│    User      │     │ ProjectAllocation│
├──────────────┤     ├──────────────────┤
│ Id (PK)      │←────│ UserId (FK)      │
│ FullName     │     │ ProjectId (FK)   │
│ Email        │     │ Role             │
│ Password     │     └────────┬─────────┘
│ RoleId (FK)──┘              │
│ CreatedProjects│            │
│ AssignedTasks │             │
└──────┬───────┘              │
       │                      │
       ▼                      ▼
┌──────────────┐     ┌──────────────────┐
│   Project    │────→│  ProjectTask (Tasks)│
├──────────────┤     ├──────────────────┤
│ Id (PK)      │     │ Id (PK)          │
│ Title        │     │ Title            │
│ Description  │     │ Description      │
│ StartDate    │     │ Deadline         │
│ EndDate      │     │ Status           │
│ Status       │     │ ProjectId (FK)───│
│ CreatedBy    │     │ AssignedTo (FK)──│
│ UserId (FK)  │     └──────────────────┘
└──────────────┘
```

Each arrow (→) is a foreign key relationship. The navigation properties in C# are what make these arrows work without writing SQL.

---

## 9. Key Concepts Checklist

| Concept | Understood? |
| --- | --- |
| DbContext = the database | ☐ |
| DbSet = a table | ☐ |
| Migration = a version of the schema | ☐ |
| `Up()` = apply, `Down()` = undo | ☐ |
| FK property + Navigation property = relationship | ☐ |
| Code-First = models → database | ☐ |
| Database-First = database → models | ☐ |

---

## Summary

You now have a full Student Project Management schema:

- **Who** — Users, Roles, Permissions (identity & access)
- **What** — Projects, Tasks (work items)
- **Who does what** — ProjectAllocation (assign users to projects)

All managed through EF Core Code-First with full migration history.
