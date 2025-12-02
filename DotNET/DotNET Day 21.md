# 🗓️ Day 21 – Asynchronous Programming with EF Core

## 🎯 Goal for Today
Understand how **asynchronous programming** works in C# and **why EF Core APIs use async methods** for database calls.

You’ll learn:
- What are `Task`, `async`, and `await`
- How async improves scalability in Web APIs
- Async EF Core methods (`ToListAsync`, `SaveChangesAsync`, etc.)
- Best practices and common pitfalls

---

## 🧠 1. Why Asynchronous Programming?

When a Web API handles many users, **I/O-bound operations** (like database calls) can slow down requests.  
If you use **synchronous code**, each request **blocks a thread** until completion.

With **asynchronous programming**, requests don’t block threads — allowing your server to handle **hundreds more requests** concurrently.

---

## ⚙️ 2. What is a Task in C#?

A `Task` represents a **unit of work** that runs **asynchronously** — it might complete **now, later, or never**.

Example:
```csharp
Task<int> task = SomeLongRunningOperationAsync();
int result = await task;
```

It’s like saying:
🗣️ “Start this work in the background, and I’ll come back when it’s done.”

## 🧩 3. The async and await Keywords
async

Used to mark a method that performs asynchronous work.

await

Suspends the execution until the awaited Task completes.

Example:

```csharp
public async Task<int> AddAsync(int a, int b)
{
    await Task.Delay(2000); // Simulate long operation
    return a + b;
}
```

## ⚙️ 4. Synchronous vs Asynchronous Example
❌ Synchronous

```csharp
public IActionResult GetEmployees()
{
    var employees = _context.Employees.ToList(); // blocks thread
    return Ok(employees);
}
```

✅ Asynchronous
```
public async Task<IActionResult> GetEmployees()
{
    var employees = await _context.Employees.ToListAsync(); // non-blocking
    return Ok(employees);
}
```

EF Core executes SQL queries asynchronously under the hood, freeing up server threads.

## 🧩 5. Common Async EF Core Methods

| Operation       | Synchronous        | Asynchronous            |
| --------------- | ------------------ | ----------------------- |
| Get all records | `ToList()`         | `ToListAsync()`         |
| Find record     | `FirstOrDefault()` | `FirstOrDefaultAsync()` |
| Check existence | `Any()`            | `AnyAsync()`            |
| Count records   | `Count()`          | `CountAsync()`          |
| Save changes    | `SaveChanges()`    | `SaveChangesAsync()`    |

✅ Always use the async variant inside Web APIs.

## ⚙️ 6. Example: Async CRUD Controller

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
    public async Task<IActionResult> GetAll()
    {
        var employees = await _context.Employees.ToListAsync();
        return Ok(employees);
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetById(int id)
    {
        var emp = await _context.Employees.FindAsync(id);
        return emp == null ? NotFound() : Ok(emp);
    }

    [HttpPost]
    public async Task<IActionResult> Add(Employee employee)
    {
        await _context.Employees.AddAsync(employee);
        await _context.SaveChangesAsync();
        return CreatedAtAction(nameof(GetById), new { id = employee.Id }, employee);
    }

    [HttpPut("{id}")]
    public async Task<IActionResult> Update(int id, Employee updated)
    {
        var emp = await _context.Employees.FindAsync(id);
        if (emp == null) return NotFound();

        emp.Name = updated.Name;
        emp.Department = updated.Department;
        emp.Salary = updated.Salary;

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

## ⚙️ 7. How Async Helps Scalability

Let’s compare:

| Feature                  | Synchronous  | Asynchronous       |
| ------------------------ | ------------ | ------------------ |
| Thread blocked           | ✅ Yes        | ❌ No            |
| Handles concurrent users | 🚫 Poor      | ✅ Excellent      |
| Resource usage           | 🧱 High      | 💨 Efficient      |
| Typical for              | Console apps | Web APIs, DB calls |

## 🧩 8. Async Flow Visualization

```text
Client → API → async DB call → (thread released)
                           ↓
                     DB finishes
                           ↓
                Thread resumes → returns response
```
So your app can handle new requests while waiting for DB responses.

## 🧩 11. Async + LINQ

You can use async methods with LINQ queries:

```csharp
var highPaid = await _context.Employees
    .Where(e => e.Salary > 80000)
    .OrderBy(e => e.Name)
    .ToListAsync();
```

✅ The query runs asynchronously, returning a Task<List<Employee>>.

## ⚙️ 12. Handling Multiple Async Tasks

Use Task.WhenAll() to run multiple queries concurrently.

Example:

```csharp
var employeesTask = _context.Employees.ToListAsync();
var departmentsTask = _context.Departments.ToListAsync();

await Task.WhenAll(employeesTask, departmentsTask);

var employees = employeesTask.Result;
var departments = departmentsTask.Result;
```

⚡ Both queries run simultaneously instead of sequentially.

## 🧠 13. Mixing Async & Sync Code Carefully

If your method is CPU-bound (e.g., processing or calculations), async won’t help.
Async is best for I/O-bound operations like:

1. DB access
2. API calls
3. File I/O