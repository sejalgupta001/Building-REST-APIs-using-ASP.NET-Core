# Implementing File Upload, Update, and Deletion
<img width="1672" height="941" alt="ChatGPT Image Sep 11, 2026, 08_35_12 AM" src="https://github.com/user-attachments/assets/3fa8c019-f4eb-414d-a931-c24e7b76b4dc" />

### Step 1: Create the File Service Interface and Class

Define a contract and service responsible for physically writing files to disk and deleting them when requested.

```csharp
// IFileService.cs
public interface IFileService
{
    Task<string> UploadFileAsync(IFormFile file, string subFolder);
    void DeleteFile(string? relativePath);
}

// FileService.cs
public class FileService : IFileService
{
    private readonly string _webRootPath;
    private const string FilesBaseFolder = "Files";

    public FileService(IWebHostEnvironment environment)
    {
        // Fallback to ContentRootPath/wwwroot if WebRootPath is null
        _webRootPath = environment.WebRootPath
            ?? Path.Combine(environment.ContentRootPath, "wwwroot");

        if (!Directory.Exists(_webRootPath))
        {
            Directory.CreateDirectory(_webRootPath);
        }
    }

    public async Task<string> UploadFileAsync(IFormFile file, string subFolder)
    {
        if (file == null || file.Length == 0)
            throw new ArgumentException("File is empty.");

        string uploadFolderPath = Path.Combine(_webRootPath, FilesBaseFolder, subFolder);

        if (!Directory.Exists(uploadFolderPath))
        {
            Directory.CreateDirectory(uploadFolderPath);
        }

        // Generate unique filename to prevent overwriting existing files
        string uniqueFileName = $"{Guid.NewGuid()}_{Path.GetFileName(file.FileName)}";
        string fullPhysicalPath = Path.Combine(uploadFolderPath, uniqueFileName);

        using (var stream = new FileStream(fullPhysicalPath, FileMode.Create))
        {
            await file.CopyToAsync(stream);
        }

        // Return relative path for database storage
        return Path.Combine(FilesBaseFolder, subFolder, uniqueFileName).Replace("\\", "/");
    }

    public void DeleteFile(string? relativePath)
    {
        if (string.IsNullOrWhiteSpace(relativePath)) return;

        string fullPath = Path.Combine(_webRootPath, relativePath.TrimStart('/', '\\'));

        if (File.Exists(fullPath))
        {
            try
            {
                File.Delete(fullPath);
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error deleting file {fullPath}: {ex.Message}");
            }
        }
    }
}

```

---

### Step 2: Define the DTO for File Uploads

Use `IFormFile` to accept binary file data from HTTP forms.

```csharp
// UserDTO.cs
public class UserDTO
{
    public int UserId { get; set; }
    public string UserName { get; set; }
    public string Password { get; set; }

    // Maps to multipart/form-data input field
    public IFormFile? DocumentFile { get; set; }
}

```

---

### Step 3: Implement Controller Endpoint Logic

Update controller action methods to bind form data and invoke `IFileService`.

```csharp
// UserController.cs

// CREATE: api/User
[HttpPost]
[Consumes("multipart/form-data")]
public async Task<IActionResult> Create([FromForm] UserDTO dto)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);

    string? uploadedPath = null;
    if (dto.DocumentFile != null)
    {
        uploadedPath = await _fileService.UploadFileAsync(dto.DocumentFile, "Users");
    }

    var user = new UserModel
    {
        UserName = dto.UserName,
        Password = dto.Password,
        DocumentPath = uploadedPath // Save relative path to DB
    };

    await _dbcontext.Users.AddAsync(user);
    await _dbcontext.SaveChangesAsync();

    return Ok(user);
}

// UPDATE: api/User/5
[HttpPut("{id:int}")]
[Consumes("multipart/form-data")]
public async Task<IActionResult> Update(int id, [FromForm] UserDTO dto)
{
    if (id != dto.UserId)
        return BadRequest("ID mismatch.");

    var existingUser = await _dbcontext.Users.FindAsync(id);
    if (existingUser == null)
        return NotFound("User not found.");

    if (dto.DocumentFile != null && dto.DocumentFile.Length > 0)
    {
        // 1. Delete physical file on disk
        _fileService.DeleteFile(existingUser.DocumentPath);

        // 2. Upload replacement file and update path
        existingUser.DocumentPath = await _fileService.UploadFileAsync(dto.DocumentFile, "Users");
    }

    existingUser.UserName = dto.UserName;
    if (!string.IsNullOrWhiteSpace(dto.Password))
    {
        existingUser.Password = dto.Password;
    }

    _dbcontext.Users.Update(existingUser);
    await _dbcontext.SaveChangesAsync();

    return Ok(existingUser);
}

// DELETE: api/User/5
[HttpDelete("{id:int}")]
public async Task<IActionResult> Delete(int id, [FromQuery] bool deleteFileOnly = false)
{
    var user = await _dbcontext.Users.FindAsync(id);
    if (user == null)
        return NotFound("User not found.");

    if (deleteFileOnly)
    {
        if (string.IsNullOrEmpty(user.DocumentPath))
            return BadRequest("No document exists for this user.");

        _fileService.DeleteFile(user.DocumentPath);
        user.DocumentPath = null;
        await _dbcontext.SaveChangesAsync();

        return Ok("Document deleted successfully.");
    }

    // Delete both physical file and DB record
    _fileService.DeleteFile(user.DocumentPath);
    _dbcontext.Users.Remove(user);
    await _dbcontext.SaveChangesAsync();

    return NoContent();
}

```

---

### Step 4: Configure `Program.cs` for File Uploads

Register services, configure file payload size limits, set up `wwwroot`, and enable static file middleware.

```csharp
// Program.cs

// 1. Register Service
builder.Services.AddScoped<IFileService, FileService>();

// 2. Configure file upload body length limits (e.g., 15 MB)
builder.Services.Configure<FormOptions>(options =>
{
    options.MultipartBodyLengthLimit = 15 * 1024 * 1024;
});

// ... database & auth registrations ...

var app = builder.Build();

// 3. MUST enable Static Files to make uploaded files accessible via browser URL
app.UseStaticFiles();

app.MapControllers();
app.Run();

```

---
### POST User API : Make sure you select multipart/form-data under Body when sending the request.
<img width="1527" height="727" alt="image" src="https://github.com/user-attachments/assets/3ec3bcad-6ce4-4fae-b90f-c4b85a3a0210" />


### Delete User API : Use the `deleteFileOnly` query parameter to choose what you want to delete:

 - Set **`deleteFileOnly=true`** if you only want to delete the user's uploaded file/document. The user record will remain in the database.
- Set **`deleteFileOnly=false`** if you want to delete the **entire user record/data** from the database. The associated physical file will also be deleted.

 **Examples:**

 `DELETE /api/User/5?deleteFileOnly=true`\
 → Deletes **only the document/file**.

 `DELETE /api/User/5?deleteFileOnly=false`\
 → Deletes the **entire user record and its document/file**.

 If `deleteFileOnly` is not provided, it defaults to **false**, so the entire user record and associated file will be deleted.
<img width="1392" height="616" alt="image" src="https://github.com/user-attachments/assets/a3acee87-5499-456b-af01-075c485418a3" />

### ⚠️ Common Mistakes

> **1. Using [FromBody] Instead of [FromForm]**
> * **Mistake:** Decorating file endpoint DTOs with `[FromBody]` causes HTTP 415 Unsupported Media Type or NULL models.
> * **Fix:** File uploads require `[FromForm]` and `[Consumes("multipart/form-data")]`.
> 
> 

> **2. Forgetting app.UseStaticFiles()**
> * **Mistake:** Saving files successfully to `wwwroot`, but browser requests to `http://localhost:port/Files/Users/file.png` return `404 Not Found`.
> * **Fix:** Ensure `app.UseStaticFiles()` is called in `Program.cs` before mapping routes.
> 
> 

> **3. Storing Full Absolute Paths in the Database**
> * **Mistake:** Saving `C:\Users\Student\Project\wwwroot\Files\image.jpg` into the database.
> * **Fix:** Save relative paths (`Files/Users/image.jpg`). Absolute paths fail completely when deploying to servers, Linux containers, or cloud environments.
> 
> 

> **4. Overwriting Files with Identical Names**
> * **Mistake:** Saving uploaded files directly using `file.FileName`. If two users upload `resume.pdf`, one overwrites the other.
> * **Fix:** Always prepend a unique identifier: `${Guid.NewGuid()}_${file.FileName}`.
> 
> 

> **5. Orphaned Files on Delete or Update**
> * **Mistake:** Calling `_dbcontext.Users.Remove(user)` without deleting the file from disk first, leading to wasted server disk space.
> * **Fix:** Call `_fileService.DeleteFile(user.DocumentPath)` before removing the database entity or replacing an old file.
> 
> 

> **6. NullReferenceException` on `builder.Environment.WebRootPath**
> * **Mistake:** Accessing `WebRootPath` when no `wwwroot` folder exists physically in the project directory causing `WebRootPath` to be `null`.
> * **Fix:** Explicitly check and create `wwwroot` in `Program.cs` and set `builder.Environment.WebRootPath`.
> 
>
