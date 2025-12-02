# 🗓️ Day 20 – LINQ with Entity Framework Core

## 🎯 Goal for Today
Learn how to use **Language Integrated Query (LINQ)** with **Entity Framework Core** to query, filter, sort, group, and join your database data using clean C# syntax.

---

## 🧠 1. What is LINQ?

**LINQ (Language Integrated Query)** allows you to query data directly from C# objects, databases, XML, and more using a uniform syntax.

EF Core translates LINQ queries into **SQL** and executes them on the database.

### Example:
```csharp
var employees = _context.Employees
    .Where(e => e.Salary > 50000)
    .OrderBy(e => e.Name)
    .ToList();
```

🔹 EF Core will generate the corresponding SQL under the hood.

---

## 💡 2. LINQ Query Syntax vs Method Syntax

LINQ has two ways of writing queries:

1️⃣ Query Syntax (SQL-like)

```csharp
var result = from e in _context.Employees
             where e.Salary > 50000
             orderby e.Name
             select e;
```

2️⃣ Method Syntax (Chained Methods)

```csharp
var result = _context.Employees
    .Where(e => e.Salary > 50000)
    .OrderBy(e => e.Name)
    .ToList();
```

## 🧩 3. Basic LINQ Operations

| Operation                           | Description             | Example                           |
| ----------------------------------- | ----------------------- | --------------------------------- |
| `Where()`                           | Filter records          | `.Where(e => e.Salary > 50000)`   |
| `Select()`                          | Project specific fields | `.Select(e => e.Name)`            |
| `OrderBy()` / `OrderByDescending()` | Sort results            | `.OrderBy(e => e.JoiningDate)`    |
| `FirstOrDefault()`                  | Get first match or null | `.FirstOrDefault(e => e.Id == 1)` |
| `Any()` / `All()`                   | Boolean checks          | `.Any(e => e.Salary > 80000)`     |
| `Count()`                           | Count results           | `.Count()`                        |
| `Take(n)` / `Skip(n)`               | Pagination              | `.Skip(10).Take(10)`              |


## ⚙️ 4. Example: Query Employees

```csharp
var employees = await _context.Employees
    .Where(e => e.DepartmentId == 2 && e.Salary > 60000)
    .OrderByDescending(e => e.JoiningDate)
    .Select(e => new
    {
        e.Name,
        e.Salary,
        DepartmentName = e.Department!.Name
    })
    .ToListAsync();
```

✅ Select only required fields (projection)
✅ Include related data (Department.Name)

## 🧠 5. Using Include() for Related Data

EF Core supports Eager Loading via Include() and ThenInclude():

```csharp
var departments = await _context.Departments
    .Include(d => d.Employees)
    .ToListAsync();
```

For nested relationships:

```csharp
var courses = await _context.Courses
    .Include(c => c.Students)
    .ThenInclude(s => s.Address)
    .ToListAsync();
```

## 🔁 6. Filtering Related Data

You can filter child collections using Select():

```csharp
var departments = await _context.Departments
    .Select(d => new
    {
        d.Name,
        Employees = d.Employees
            .Where(e => e.Salary > 60000)
            .ToList()
    })
    .ToListAsync();
```

## 🔍 7. Aggregation Functions

LINQ supports aggregation similar to SQL:

```csharp
var totalEmployees = await _context.Employees.CountAsync();
var maxSalary = await _context.Employees.MaxAsync(e => e.Salary);
var avgSalary = await _context.Employees.AverageAsync(e => e.Salary);
var minSalary = await _context.Employees.MinAsync(e => e.Salary);
var sumSalary = await _context.Employees.SumAsync(e => e.Salary);
```

## 🧩 8. Grouping Data

Group records by a key (like DepartmentId):

```csharp
var grouped = await _context.Employees
    .GroupBy(e => e.Department!.Name)
    .Select(g => new
    {
        Department = g.Key,
        Count = g.Count(),
        AverageSalary = g.Average(e => e.Salary)
    })
    .ToListAsync();
```

✅ Useful for reports or dashboards (like “Employee count by department”).

## 🧠 9. Joins in LINQ

You can join tables manually (though EF usually handles via navigation properties).

Example: Inner Join

```csharp
var query = from e in _context.Employees
            join d in _context.Departments
            on e.DepartmentId equals d.Id
            select new
            {
                EmployeeName = e.Name,
                DepartmentName = d.Name
            };
```

Example: Left Join

```csharp
var query = from e in _context.Employees
            join d in _context.Departments
            on e.DepartmentId equals d.Id into deptGroup
            from dept in deptGroup.DefaultIfEmpty()
            select new
            {
                e.Name,
                DepartmentName = dept != null ? dept.Name : "No Department"
            };
```

## ⚙️ 10. Combining LINQ Queries

LINQ supports chaining for complex filtering:

```csharp
var recentHighPaid = await _context.Employees
    .Where(e => e.Salary > 80000)
    .Where(e => e.JoiningDate > new DateTime(2020, 1, 1))
    .OrderBy(e => e.Name)
    .ToListAsync();
```

## 🧠 11. Asynchronous LINQ Methods

| Method                   | Description                   |
| ------------------------ | ----------------------------- |
| `ToListAsync()`          | Executes query asynchronously |
| `FirstOrDefaultAsync()`  | Returns first or null         |
| `SingleOrDefaultAsync()` | Throws if multiple found      |
| `CountAsync()`           | Returns count                 |
| `AnyAsync()`             | Checks if any element exists  |

```csharp
var emp = await _context.Employees.FirstOrDefaultAsync(e => e.Id == 10);
```

## ⚙️ 12. Pagination with LINQ

Used to limit large datasets (e.g., in APIs):

```csharp
int pageNumber = 2;
int pageSize = 10;

var pagedData = await _context.Employees
    .OrderBy(e => e.Id)
    .Skip((pageNumber - 1) * pageSize)
    .Take(pageSize)
    .ToListAsync();
```

## 🧩 13. Example: Complete LINQ Controller

```csharp
[ApiController]
[Route("api/[controller]")]
public class EmployeeController : ControllerBase
{
    private readonly AppDbContext _context;

    public EmployeeController(AppDbContext context)
    {
        _context = context;
    }

    [HttpGet("high-salary")]
    public async Task<IActionResult> GetHighSalaryEmployees()
    {
        var employees = await _context.Employees
            .Include(e => e.Department)
            .Where(e => e.Salary > 70000)
            .OrderByDescending(e => e.Salary)
            .Select(e => new
            {
                e.Name,
                e.Salary,
                Department = e.Department!.Name
            })
            .ToListAsync();

        return Ok(employees);
    }
}
```
✅ Combines filtering, sorting, projection, and eager loading.

## 🧠 14. LINQ-to-Entities vs LINQ-to-Objects

| Type             | Works On                 | Executes In    | Example                         |
| ---------------- | ------------------------ | -------------- | ------------------------------- |
| LINQ-to-Entities | EF Core entities (DbSet) | Database (SQL) | `_context.Employees.Where(...)` |
| LINQ-to-Objects  | In-memory collections    | C# memory      | `employeesList.Where(...)`      |

⚠️ EF Core translates only supported expressions to SQL.
Avoid calling C# methods inside LINQ that can’t be translated (e.g., Regex.IsMatch).

## 🧩 15. Common Mistakes to Avoid

❌ Mixing in-memory and DB LINQ calls incorrectly:

```csharp
var employees = _context.Employees.ToList().Where(e => e.Salary > 50000); // BAD
```

✅ Instead:

```csharp
var employees = _context.Employees.Where(e => e.Salary > 50000).ToList();
```