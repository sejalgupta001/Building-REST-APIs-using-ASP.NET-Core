## What is Pagination?

![Refresh Token](./Pagination.png)
Pagination is the process of dividing a large amount of data into smaller pages, making it easier to view and manage.

---

## Create the GetAll API with Pagination

```csharp
public async Task<IActionResult> GetAllTasks(int pageNumber = 1, int pageSize = 10)
```

- `pageNumber` tells which page to display, `pageSize` tells how many records to show on each page.
- `pageNumber = 1` → Default page is **1**.
- `pageSize = 10` → Default is **10 records per page**.

---

### Example: `GetAllTasks`

```csharp
[HttpGet]
public async Task<IActionResult> GetAllTasks(int pageNumber = 1, int pageSize = 10)
{
    try
    {
        if (pageNumber < 1 || pageSize < 1)
        {
            return BadRequest("Page number and page size must be greater than 0.");
        }
        var query = _context.SPM_Task
                            .AsNoTracking()
                            .Where(t => t.TaskStatusID != null);
        var totalCount = await query.CountAsync();
        var items = await query
            .OrderBy(t => t.TaskDueDate)
            .Skip((pageNumber - 1) * pageSize)
            .Take(pageSize)
            .ToListAsync();

        return Ok(new
        {
            items,
            pageNumber,
            pageSize,
            totalCount,
            totalPages = (int)Math.Ceiling(totalCount / (double)pageSize)
        });
    }
    catch (Exception ex)
    {
        return StatusCode(500, new
        {
            message = "Error getting tasks",
            error = ex.Message
        });
    }
}
```

---

### Applying Pagination

![Without Without](./Paging.png)

---

# Filtering

![Refresh Token](./Filtering.png)

### Why do we use Filtering?

- Display only the required task/project records.
- Improve the user experience for students and faculty.
- Reduce unnecessary data returned by the API.

---

## Step 1 — Take SPM_Task Table

| Property            | Data Type     |
| ------------------- | ------------- |
| TaskID              | int           |
| ProjectAllocationID | int           |
| TaskTitle           | nvarchar(200) |
| TaskStatusID        | int           |
| TaskPriorityID      | int           |
| AssignedScore       | decimal(5,2)  |
| TaskDueDate         | datetime      |

Sample Data

| TaskID | TaskTitle     | TaskPriorityID | TaskStatusID  | AssignedScore |
| ------ | ------------- | -------------- | ------------- | ------------- |
| 1      | Login Page    | 1 (Critical)   | 4 (Pending)   | 25            |
| 2      | Dashboard UI  | 2 (Moderate)   | 3 (Completed) | 50            |
| 3      | User CRUD     | 1 (Critical)   | 4 (Pending)   | 25            |
| 4      | Report Module | 3 (Low)        | 2 (Ongoing)   | 30            |
| 5      | API Testing   | 2 (Moderate)   | 3 (Completed) | 50            |

---

## Step 2 — Get All Tasks

Start with a simple API that returns all tasks.

```csharp
[HttpGet]
public async Task<IActionResult> GetTasks()
{
    var tasks = await _context.SPM_Task.ToListAsync();

    return Ok(tasks);
}
```

### Output

All `SPM_Task` records are returned.

---

## Step 3 — Apply Multiple Filters

### Controller Code

```csharp
[HttpGet]
public async Task<IActionResult> GetTasks(
    int? taskId,
    string? taskTitle,
    int? taskPriorityId,
    int? taskStatusId,
    decimal? assignedScore,
    DateTime? dueDate,
    DateTime? fromDate,
    DateTime? toDate)
{
    try
    {
        var query = _context.SPM_Task.AsQueryable();

        // Integer Filter
        if (taskId.HasValue)
        {
            query = query.Where(task => task.TaskID == taskId.Value);
        }

        // String Filter (Title)
        if (!string.IsNullOrWhiteSpace(taskTitle))
        {
            query = query.Where(task => task.TaskTitle.Contains(taskTitle));
        }

        // Lookup Filter (Priority)
        if (taskPriorityId.HasValue)
        {
            query = query.Where(task => task.TaskPriorityID == taskPriorityId.Value);
        }

        // Lookup Filter (Status)
        if (taskStatusId.HasValue)
        {
            query = query.Where(task => task.TaskStatusID == taskStatusId.Value);
        }

        // Decimal Filter
        if (assignedScore.HasValue)
        {
            query = query.Where(task => task.AssignedScore == assignedScore.Value);
        }

        // Date Filter
        if (dueDate.HasValue)
        {
            query = query.Where(task => task.TaskDueDate.Value.Date == dueDate.Value.Date);
        }

        // Date Range Filter
        if (fromDate.HasValue)
        {
            query = query.Where(task => task.TaskDueDate >= fromDate.Value);
        }

        if (toDate.HasValue)
        {
            query = query.Where(task => task.TaskDueDate <= toDate.Value);
        }

        var tasks = await query.ToListAsync();

        return Ok(tasks);
    }
    catch (Exception ex)
    {
        return StatusCode(500, new
        {
            message = "Something went wrong.",
            error = ex.Message
        });
    }
}
```

### Without Applying Flitering

![Without Without](<./Filtaring(Without).png>)

### Apply a Flitering (TaskPriority)

![With Without](<./Filtering(With).png>)
