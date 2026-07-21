# Migration Errors and Fixes
## 1. First migration error

![Migration error 1](https://res.cloudinary.com/dpdxoxtog/image/upload/v1784650043/IMG-20260721-WA0006_iry75j.jpg)
Error says Entity Framework cannot determine the relationship for `ProjectAllocation.Faculty` of type `User`. In other words, EF Core sees the `Faculty` navigation property and does not know how to map it during migration.

To solve that error add [ForenKey] like shown below.
![Fix for migration error 1](https://res.cloudinary.com/dpdxoxtog/image/upload/v1784650042/IMG-20260721-WA0007_ntmhez.jpg)

If the error is still showing after that, replace the `ProjectAllocation` configuration inside `OnModelCreating` with this block:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<ProjectAllocation>(entity =>
    {
        entity.ToTable("SPM_ProjectAllocation");
        entity.Property(e => e.AssignedDate)
            .HasDefaultValueSql("GETDATE()");

        entity.HasOne(e => e.Project)
            .WithMany(e => e.ProjectAllocations)
            .HasForeignKey(e => e.ProjectId);

        entity.HasOne(e => e.Student)
            .WithMany(e => e.StudentProjectAllocations)
            .HasForeignKey(e => e.StudentId);

        entity.HasOne(e => e.Faculty)
            .WithMany(e => e.FacultyProjectAllocations)
            .HasForeignKey(e => e.FacultyId);

        entity.HasMany(e => e.Tasks)
            .WithOne(e => e.ProjectAllocation)
            .HasForeignKey(e => e.ProjectAllocationId);
    });
}
```

## 2. Second migration error

![Migration error 2](https://res.cloudinary.com/dpdxoxtog/image/upload/v1784650043/IMG-20260721-WA0008_ygtthg.jpg)
The second error says SQL Server is rejecting the foreign key because the relationships may create cycles or multiple cascade paths. This happens when more than one cascade delete path can reach the same table.

To solve the problem go to the Folder find letest migration file find where is the error came from and replace `onDelete: ReferentialAction.Cascade);` with `onDelete: ReferentialAction.Restrict);` as shown in picture.

![Fix for migration error 2](https://res.cloudinary.com/dpdxoxtog/image/upload/v1784650043/IMG-20260721-WA0005_maaws8.jpg)

If you want the alternative fix in code, update the same `ProjectAllocation` block inside `OnModelCreating` in [Data/AppDbContext.cs](Data/AppDbContext.cs) with this version:

```csharp
modelBuilder.Entity<ProjectAllocation>(entity =>
{
    entity.ToTable("SPM_ProjectAllocation");

    entity.Property(e => e.AssignedDate)
        .HasDefaultValueSql("GETDATE()");

    entity.HasOne(e => e.Project)
        .WithMany(e => e.ProjectAllocations)
        .HasForeignKey(e => e.ProjectId)
        .OnDelete(DeleteBehavior.Cascade);

    entity.HasOne(e => e.Student)
        .WithMany(e => e.StudentProjectAllocations)
        .HasForeignKey(e => e.StudentId)
        .OnDelete(DeleteBehavior.Restrict);

    entity.HasOne(e => e.Faculty)
        .WithMany(e => e.FacultyProjectAllocations)
        .HasForeignKey(e => e.FacultyId)
        .OnDelete(DeleteBehavior.Restrict);

    entity.HasMany(e => e.Tasks)
        .WithOne(e => e.ProjectAllocation)
        .HasForeignKey(e => e.ProjectAllocationId)
        .OnDelete(DeleteBehavior.Cascade);
});
```