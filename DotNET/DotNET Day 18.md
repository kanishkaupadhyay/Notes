# 🗓️ Day 18 – Entity Framework Core Basics

## 🎯 Goal for Today
Learn the **fundamentals of Entity Framework Core (EF Core)** — how to:
- Connect your Web API to a database
- Define models (entities)
- Configure a `DbContext`
- Create and run database migrations
- Perform basic CRUD operations

---

## 🧠 1. What is Entity Framework Core?

**Entity Framework Core (EF Core)** is Microsoft’s **Object-Relational Mapper (ORM)** that allows you to work with databases using C# classes instead of SQL.

It handles:
- Table creation
- Relationships
- Data querying
- CRUD operations

Think of EF Core as the bridge between your **C# objects** and your **database tables**.

---

## ⚙️ 2. Installing EF Core

You’ll need two main NuGet packages for SQL Server:

```bash
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

## 🧩 3. Defining Your Model (Entity)

Let’s start simple — an Employee model.

```csharp
public class Employee
{
    public int Id { get; set; }

    public string Name { get; set; } = string.Empty;

    public string Department { get; set; } = string.Empty;

    public int Age { get; set; }
}
```
1. Each class → Table
2. Each property → Column
3. Id (or <ClassName>Id) → Primary Key by convention

## ⚙️ 4. Creating a DbContext

The DbContext is the bridge between your code and database.

```csharp
using Microsoft.EntityFrameworkCore;

public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options)
        : base(options)
    {
    }

    public DbSet<Employee> Employees { get; set; }
}
```
Key Points
1. DbSet<Employee> represents the Employees table.
2. DbContextOptions contains your connection string and provider (e.g., SQL Server).

## ⚙️ 5. Registering DbContext in Program.cs

In your Web API project, open Program.cs and register EF Core:

```csharp
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

// ✅ Add DbContext and specify connection string
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

var app = builder.Build();
app.MapControllers();
app.Run();
```

## 🧩 6. Add Connection String (in appsettings.json)

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=EmployeeDb;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```
💡 Adjust Server and authentication based on your SQL Server setup.

## ⚙️ 7. Creating and Running Migrations

Migrations are used to sync your models with the database.

Create a Migration
```bash
dotnet ef migrations add InitialCreate
```

Apply the Migration
```bash
dotnet ef database update
```
✅ This creates the EmployeeDb database and an Employees table.

## 🧩 8. Basic CRUD Operations

```csharp
[ApiController]
[Route("api/[controller]")]
public class EmployeesController : ControllerBase
{
    private readonly AppDbContext _context;

    public EmployeesController(AppDbContext context)
    {
        _context = context;
    }

    [HttpGet]
    public async Task<IActionResult> GetAll() =>
        Ok(await _context.Employees.ToListAsync());

    [HttpGet("{id}")]
    public async Task<IActionResult> Get(int id)
    {
        var emp = await _context.Employees.FindAsync(id);
        return emp == null ? NotFound() : Ok(emp);
    }

    [HttpPost]
    public async Task<IActionResult> Create(Employee emp)
    {
        _context.Employees.Add(emp);
        await _context.SaveChangesAsync();
        return CreatedAtAction(nameof(Get), new { id = emp.Id }, emp);
    }

    [HttpPut("{id}")]
    public async Task<IActionResult> Update(int id, Employee updated)
    {
        var emp = await _context.Employees.FindAsync(id);
        if (emp == null) return NotFound();

        emp.Name = updated.Name;
        emp.Department = updated.Department;
        emp.Age = updated.Age;

        await _context.SaveChangesAsync();
        return Ok(emp);
    }

    [HttpDelete("{id}")]
    public async Task<IActionResult> Delete(int id)
    {
        var emp = await _context.Employees.FindAsync(id);
        if (emp == null) return NotFound();

        _context.Employees.Remove(emp);
        await _context.SaveChangesAsync();
        return NoContent();
    }
}
```

## ⚙️ 10. Verifying the Database

After running the app and performing CRUD operations:

```sql
SELECT * FROM Employees;
```
You’ll see data inserted directly by EF Core via your API calls 🎉

## 🧠 11. Understanding EF Core Change Tracking

EF Core keeps track of object states:

1. Added → new entities to insert
2. Modified → entities to update
3. Deleted → entities to delete
4. Unchanged → no changes

When you call:

```csharp
await _context.SaveChangesAsync();
```
EF Core automatically generates and executes the necessary SQL commands.

## 🧩 12. EF Core Lifecycle

| Step | Description                 |
| ---- | --------------------------- |
| 1️⃣  | Define Models               |
| 2️⃣  | Create DbContext            |
| 3️⃣  | Configure Connection String |
| 4️⃣  | Create Migration            |
| 5️⃣  | Update Database             |
| 6️⃣  | Use EF Core for CRUD        |