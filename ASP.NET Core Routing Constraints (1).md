# ASP.NET Core Routing Constraints

## 1. What is a Routing Constraint?

A **routing constraint** restricts the value of a route parameter.

### Syntax

```csharp
{parameter:constraint}
```

Example:

```csharp
[HttpGet("product/{id:int}")]
```

Here, `id` must be an integer.

---

# 2. Common Built-in Constraints

| Constraint  | Example                | Purpose               |
| ----------- | ---------------------- | --------------------- |
| `int`       | `{id:int}`             | Integer               |
| `long`      | `{id:long}`            | Long integer          |
| `bool`      | `{status:bool}`        | True/False            |
| `guid`      | `{id:guid}`            | GUID                  |
| `decimal`   | `{price:decimal}`      | Decimal               |
| `double`    | `{value:double}`       | Double                |
| `float`     | `{value:float}`        | Float                 |
| `datetime`  | `{date:datetime}`      | DateTime              |
| `min`       | `{age:min(18)}`        | Minimum value         |
| `max`       | `{qty:max(100)}`       | Maximum value         |
| `range`     | `{score:range(0,100)}` | Value within range    |
| `alpha`     | `{name:alpha}`         | Letters only          |
| `minlength` | `{name:minlength(3)}`  | Minimum string length |
| `maxlength` | `{name:maxlength(10)}` | Maximum string length |
| `length`    | `{code:length(2,5)}`   | String length range   |
| `regex`     | `{code:regex(...)}`    | Pattern matching      |

---

# 3. `int` Constraint

Only integer values are allowed.

```csharp
[HttpGet("product/{id:int}")]
public IActionResult GetProduct(int id)
{
    return Ok($"Product ID: {id}");
}
```

Valid:

```text
/product/10
```

Invalid:

```text
/product/abc
```

---

# 4. `range` Constraint

Used to restrict a numeric value to a range.

```csharp
[HttpGet("marks/{score:range(0,100)}")]
public IActionResult GetMarks(int score)
{
    return Ok($"Marks: {score}");
}
```

Valid:

```text
/marks/85
```

Invalid:

```text
/marks/150
```

---

# 5. `alpha` Constraint

Allows only alphabetic characters.

```csharp
[HttpGet("student/{name:alpha}")]
public IActionResult GetStudent(string name)
{
    return Ok($"Student: {name}");
}
```

Valid:

```text
/student/Rahul
```

Invalid:

```text
/student/Rahul123
```

---

# 6. `guid` Constraint

Used when the route parameter must be a GUID.

```csharp
[HttpGet("order/{id:guid}")]
public IActionResult GetOrder(Guid id)
{
    return Ok($"Order ID: {id}");
}
```

Valid:

```text
/order/550e8400-e29b-41d4-a716-446655440000
```

Invalid:

```text
/order/123
```

---

# 7. `regex` Constraint

Used when a parameter must follow a specific pattern.

Example: **2 uppercase letters + 4 digits**

```csharp
[HttpGet("user/{code:regex(^[A-Z]{2}[0-9]{4}$)}")]
public IActionResult GetUser(string code)
{
    return Ok($"User Code: {code}");
}
```

Valid:

```text
/user/AB1234
```

Invalid:

```text
/user/A123
```

---

# 8. Other Useful Constraints

### Minimum value

```csharp
{age:min(18)}
```

Accepts `18` or greater.

### Maximum value

```csharp
{qty:max(100)}
```

Accepts `100` or less.

### Minimum string length

```csharp
{name:minlength(3)}
```

### Maximum string length

```csharp
{name:maxlength(10)}
```

### String length range

```csharp
{code:length(2,5)}
```

---

# 9. Multiple Constraints

We can apply more than one constraint.

```csharp
[HttpGet("product/{id:int:min(1)}")]
public IActionResult GetProduct(int id)
{
    return Ok($"Product ID: {id}");
}
```

This means:

```text
id must be an integer
AND
id must be >= 1
```

---

# 10. Routing Constraint vs Validation

| Routing Constraint              | Validation                              |
| ------------------------------- | --------------------------------------- |
| Checks route values             | Checks input data                       |
| Used during route matching      | Used after data reaches the application |
| Example: `{id:int}`             | Example: `[Required]`                   |
| Controls which endpoint matches | Checks whether data is valid            |
