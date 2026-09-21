# Services & Repository Pattern in ASP.NET Core

![Service&Repository Daigram](./Repo%20&%20Services.jpeg)



<img width="1214" height="217" alt="Service Repo" src="https://github.com/user-attachments/assets/218b6b7c-7ae8-4474-b894-7e4d1fc35828" />


## Why is this Service/Repository Required?

1. **Separation of Concerns** – Separates HTTP, business, and database logic.
2. **Loose Coupling** – Reduces dependency between Controller and Database.
3. **Easy Testing & Maintenance** – Makes code easier to test, reuse, and maintain.

## Common Issues Without Service/Repository Pattern

1. **Tight Coupling** – Controller becomes directly dependent on EF Core and database implementation.
2. **Difficult Unit Testing** – Direct database dependencies make Controllers harder to mock and test.
3. **Scalability Issues** – As the application grows, Controllers become larger and harder to manage.
4. **Poor Reusability** – Business logic inside Controllers is difficult to reuse in other parts of the application.


## When to Use Service/Repository Pattern?

Use it mainly for **medium/large projects** with complex business logic.  
It is useful for **easy testing, maintenance, and scalability**.  
Examples: **E-commerce, Banking, Hospital, ERP, HR Management** systems.

### Example: Current Code Like `UserController/Add()`

```csharp
 [HttpPost]
 public async Task<IActionResult> Add(UserDto dto)
 {
    var validationResult = await _userValidator.ValidateAsync(dto);
    if (!validationResult.IsValid)
    {
        return BadRequest(validationResult.Errors);
    }
    try
    {
        var entity = new User()
        {
            UserTypeID = (int)dto.UserTypeID,
            FullName = dto.FullName,
            UserCode = dto.UserCode,
            Email = dto.Email,
            Password = dto.Password,
            MobileNumber = dto.MobileNumber,
            ProfilePicturePath = dto.ProfilePicturePath,
            IsActive = true,
            IsDeleted = false
        };
        _context.Users.Add(entity);
        _context.SaveChanges();
        return Ok(new { Status = "Success", Message = "Record Inserted" });
    }
    catch (Exception ex)
    {
        var message = ex.Message;
        return Ok(message);
    }
 }

```

### Convert to Service/Repository

#### Step - 1 Add the New Folder Repository/UserRepository

```csharp
public interface IUserRepository
{
    Task AddAsync(User user);
}
```

```csharp
public class UserRepository : IUserRepository
{
    private readonly AppDbContext _context;

    public UserRepository(AppDbContext context)
    {
        _context = context;
    }

    public async Task AddAsync(User user)
    {
        await _context.Users.AddAsync(user);
        await _context.SaveChangesAsync();
    }
}
```

#### Step - 2 Add the New Folder Service/UserService

```csharp
public interface IUserService
{
    Task<string> AddAsync(UserDto dto);
}
```

```csharp
public class UserService : IUserService
{
    private readonly IUserRepository _userRepository;

    public UserService(IUserRepository userRepository)
    {
        _userRepository = userRepository;
    }

    public async Task<string> AddAsync(UserDto dto)
    {
        var user = new User
        {
            UserTypeID = (int)dto.UserTypeID,
            FullName = dto.FullName,
            UserCode = dto.UserCode,
            Email = dto.Email,
            Password = dto.Password,
            MobileNumber = dto.MobileNumber,
            ProfilePicturePath = dto.ProfilePicturePath,
            IsActive = true,
            IsDeleted = false
        };

        await _userRepository.AddAsync(user);

        return "Record Inserted";
    }
}
```

#### Step - 3 Update UserController

```csharp
 [ApiController]
 [Route("api/[controller]/[action]")]

 public class UserController : ControllerBase
 {
  private readonly IUserService _userService;
  private readonly UserValidator _userValidator;
  public UserController(UserValidator userValidator, IUserService userService)
  {
      _userValidator = userValidator;
      _userService = userService;
  }
  [HttpPost]
  public async Task<IActionResult> Add(UserDto dto)
  {
      var validationResult = await _userValidator.ValidateAsync(dto);
      if (!validationResult.IsValid)
          return BadRequest(validationResult.Errors);

      var message = await _userService.AddAsync(dto);
      return Ok(new { Message = message });
  }
 }
```

#### Step - 4 Register Interfaces in Dependency Injection

```csharp
builder.Services.AddScoped<IUserRepository, UserRepository>();
builder.Services.AddScoped<IUserService, UserService>();
```

