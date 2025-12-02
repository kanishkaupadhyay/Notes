# 🗓️ Day 19 – Relationships in Entity Framework Core

## 🎯 Goal for Today
Learn how **Entity Framework Core** manages relationships between entities:
- One-to-One
- One-to-Many
- Many-to-Many  
and how to configure them using **navigation properties** and **foreign keys**.

---

## 🧠 1. What is a Relationship in EF Core?

In relational databases:
- A **relationship** connects data between tables using foreign keys.
- EF Core allows you to model those relationships directly using **C# navigation properties**.

Types of relationships:
| Relationship | Example | Description |
|---------------|----------|--------------|
| One-to-One | Person → Passport | One record relates to exactly one other record |
| One-to-Many | Department → Employees | One record relates to many others |
| Many-to-Many | Student ↔ Course | Multiple records relate to multiple others |

---

## 🧩 2. One-to-Many Relationship

### Example: Department → Employees

- One department can have many employees  
- Each employee belongs to one department

### Entity Models

```csharp
public class Department
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;

    // Navigation property
    public ICollection<Employee> Employees { get; set; } = new List<Employee>();
}

public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;

    // Foreign key
    public int DepartmentId { get; set; }

    // Navigation property
    public Department? Department { get; set; }
}
```

Configure in AppDbContext

```csharp
public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

    public DbSet<Employee> Employees => Set<Employee>();
    public DbSet<Department> Departments => Set<Department>();
}
```

EF Core automatically detects the relationship because:

1. Department has a collection of Employees
2. Employee has a DepartmentId foreign key and navigation property

Migration Output (Generated SQL)

```csharp
CREATE TABLE Departments (
    Id INT PRIMARY KEY IDENTITY,
    Name NVARCHAR(MAX)
);

CREATE TABLE Employees (
    Id INT PRIMARY KEY IDENTITY,
    Name NVARCHAR(MAX),
    DepartmentId INT NOT NULL,
    FOREIGN KEY (DepartmentId) REFERENCES Departments(Id)
);
```

Example Controller (Query with Relationships)

```csharp
[HttpGet("departments")]
public async Task<IActionResult> GetDepartments()
{
    var departments = await _context.Departments
        .Include(d => d.Employees)
        .ToListAsync();
    return Ok(departments);
}
```
✅ Include() eagerly loads related data (Employees with Departments).

## 🧩 3. One-to-One Relationship

Example: Each employee has one address.

```csharp
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;

    public EmployeeAddress? Address { get; set; }
}

public class EmployeeAddress
{
    public int Id { get; set; }
    public string Street { get; set; } = string.Empty;
    public string City { get; set; } = string.Empty;

    public int EmployeeId { get; set; }
    public Employee? Employee { get; set; }
}
```

Configure Relationship (Using Fluent API)

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Employee>()
        .HasOne(e => e.Address)
        .WithOne(a => a.Employee)
        .HasForeignKey<EmployeeAddress>(a => a.EmployeeId);
}
```

Migration SQL Output

```csharp
CREATE TABLE Employees (
    Id INT PRIMARY KEY IDENTITY,
    Name NVARCHAR(MAX)
);

CREATE TABLE EmployeeAddresses (
    Id INT PRIMARY KEY IDENTITY,
    Street NVARCHAR(MAX),
    City NVARCHAR(MAX),
    EmployeeId INT NOT NULL,
    FOREIGN KEY (EmployeeId) REFERENCES Employees(Id)
);
```

## 🧩 4. Many-to-Many Relationship

Example: Students and Courses
A student can take many courses, and a course can have many students.

Entities

```csharp
public class Student
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;

    public ICollection<Course> Courses { get; set; } = new List<Course>();
}

public class Course
{
    public int Id { get; set; }
    public string Title { get; set; } = string.Empty;

    public ICollection<Student> Students { get; set; } = new List<Student>();
}
```

EF Core Auto Creates a Join Table

You don’t have to define it manually unless you want custom columns.

Migration SQL:
```csharp
CREATE TABLE CourseStudent (
    CoursesId INT NOT NULL,
    StudentsId INT NOT NULL,
    FOREIGN KEY (CoursesId) REFERENCES Courses(Id),
    FOREIGN KEY (StudentsId) REFERENCES Students(Id),
    PRIMARY KEY (CoursesId, StudentsId)
);
```

Example Controller
```csharp
[HttpPost("enroll")]
public async Task<IActionResult> EnrollStudent(int studentId, int courseId)
{
    var student = await _context.Students.Include(s => s.Courses)
        .FirstOrDefaultAsync(s => s.Id == studentId);
    var course = await _context.Courses.FindAsync(courseId);

    if (student == null || course == null) return NotFound();

    student.Courses.Add(course);
    await _context.SaveChangesAsync();

    return Ok("Student enrolled successfully!");
}
```

## 🧠 5. Loading Related Data

EF Core offers 3 ways to load related data:

| Type                 | Method                                                     | Description                     |
| -------------------- | ---------------------------------------------------------- | ------------------------------- |
| **Eager Loading**    | `Include()`                                                | Loads related data immediately  |
| **Explicit Loading** | `context.Entry(entity).Collection(e => e.Related).Load();` | Load manually after main entity |
| **Lazy Loading**     | Auto-loads when property accessed *(requires config)*      |                                 |

Example (Eager Loading):
```csharp
var emp = await _context.Employees
    .Include(e => e.Department)
    .ToListAsync();
```

## ⚙️ 6. Cascade Delete

Cascade delete removes dependent entities when the parent is deleted.

Example:
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Department>()
        .HasMany(d => d.Employees)
        .WithOne(e => e.Department)
        .OnDelete(DeleteBehavior.Cascade);
}
```

✅ When a Department is deleted → all related Employees are deleted too.

Other options:

1. Restrict → Prevent deletion if related data exists
2. SetNull → Set FK to null when parent deleted

## 🧩 7. Relationship Conventions

EF Core automatically infers relationships when:

1. There’s a property like DepartmentId
2. And a navigation property like Department

You can override defaults using:

1. Fluent API (OnModelCreating)
2. Data Annotations

## ⚙️ 8. Data Annotations for Relationships

You can define simple relationships with attributes:

```csharp
public class Employee
{
    public int Id { get; set; }

    [ForeignKey("Department")]
    public int DepartmentId { get; set; }

    [Required]
    public Department? Department { get; set; }
}
```

## 9. Summary of Relationship Types

| Relationship | Example                | Configuration          |
| ------------ | ---------------------- | ---------------------- |
| One-to-One   | Employee → Address     | `HasOne().WithOne()`   |
| One-to-Many  | Department → Employees | `HasMany().WithOne()`  |
| Many-to-Many | Student ↔ Course       | `HasMany().WithMany()` |

## 🧩 10. Example: Complete Context with All Relationships

```csharp
public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

    public DbSet<Employee> Employees => Set<Employee>();
    public DbSet<Department> Departments => Set<Department>();
    public DbSet<Student> Students => Set<Student>();
    public DbSet<Course> Courses => Set<Course>();
    public DbSet<EmployeeAddress> EmployeeAddresses => Set<EmployeeAddress>();

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // One-to-One
        modelBuilder.Entity<Employee>()
            .HasOne(e => e.Address)
            .WithOne(a => a.Employee)
            .HasForeignKey<EmployeeAddress>(a => a.EmployeeId);

        // One-to-Many
        modelBuilder.Entity<Department>()
            .HasMany(d => d.Employees)
            .WithOne(e => e.Department)
            .HasForeignKey(e => e.DepartmentId)
            .OnDelete(DeleteBehavior.Cascade);

        // Many-to-Many
        modelBuilder.Entity<Student>()
            .HasMany(s => s.Courses)
            .WithMany(c => c.Students);
    }
}
```