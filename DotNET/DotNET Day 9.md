# 🗓️ Day 9 – LINQ (Language Integrated Query) in C#

## 🎯 Goal for Today
Learn to query and transform data easily using **LINQ** with both **Query** and **Method syntax**, mastering filtering, sorting, grouping, projections, and aggregations.

---

## ⚙️ 1. What is LINQ?

**LINQ (Language Integrated Query)** is a feature introduced in **.NET 3.5** that lets you query collections, databases, XML, and more — directly in C# syntax.

✅ Unified querying experience across:
- Objects in memory (`LINQ to Objects`)
- Databases (`LINQ to SQL`, `Entity Framework`)
- XML (`LINQ to XML`)
- JSON and more

Example:

```csharp
int[] numbers = { 1, 2, 3, 4, 5 };

// Query Syntax
var evenNumbers = from n in numbers
                  where n % 2 == 0
                  select n;

// Method Syntax
var evenNumbers2 = numbers.Where(n => n % 2 == 0);
```

💡 Query syntax looks like SQL.
💡 Method syntax uses extension methods (Where, Select, etc.).

## ⚙️ 2. LINQ Namespaces

To use LINQ, include:

```csharp
using System.Linq;
```

## ⚙️ 3. LINQ Query Syntax Structure

```csharp
var result = from element in collection
             where condition
             orderby element.Property
             select element;
```

Example:

```csharp
List<int> numbers = new() { 10, 5, 20, 15 };

var sortedNumbers = from n in numbers
                    where n > 10
                    orderby n
                    select n;

foreach (var n in sortedNumbers)
    Console.WriteLine(n); // 15, 20
```

## ⚙️ 4. LINQ Method Syntax

Equivalent to above:

```csharp
var sortedNumbers = numbers
                    .Where(n => n > 10)
                    .OrderBy(n => n);
```

## ⚙️ 5. Common LINQ Methods

| Method                                            | Description              | Example                     |
| ------------------------------------------------- | ------------------------ | --------------------------- |
| `Where()`                                         | Filters elements         | `Where(x => x > 10)`        |
| `Select()`                                        | Projects/transforms data | `Select(x => x * 2)`        |
| `OrderBy()`                                       | Sorts ascending          | `OrderBy(x => x)`           |
| `OrderByDescending()`                             | Sorts descending         | `OrderByDescending(x => x)` |
| `ThenBy()`                                        | Secondary sort           | `ThenBy(x => x.Name)`       |
| `GroupBy()`                                       | Groups elements          | `GroupBy(x => x.Category)`  |
| `Join()`                                          | Joins two sequences      | `Join(...)`                 |
| `Distinct()`                                      | Removes duplicates       | `Distinct()`                |
| `Take()`                                          | Takes first N elements   | `Take(5)`                   |
| `Skip()`                                          | Skips first N elements   | `Skip(2)`                   |
| `Sum()`, `Count()`, `Average()`, `Min()`, `Max()` | Aggregates               | `numbers.Sum()`             |

## ⚙️ 6. LINQ on Collections
Example: Filtering and Projecting

```csharp
List<string> names = new() { "John", "Jane", "Jack", "Jill", "Steve" };

var shortNames = names
    .Where(n => n.Length <= 4)
    .Select(n => n.ToUpper());

foreach (var name in shortNames)
    Console.WriteLine(name); // JOHN, JACK, JILL
```

Example: Sorting

```csharp
var sorted = names.OrderBy(n => n);
var sortedDesc = names.OrderByDescending(n => n);
```

Example: Grouping

```csharp
var people = new List<(string Name, string City)>
{
    ("Alice", "London"),
    ("Bob", "Paris"),
    ("Charlie", "London"),
    ("David", "Paris"),
    ("Eve", "New York")
};

var groups = people.GroupBy(p => p.City);

foreach (var group in groups)
{
    Console.WriteLine($"City: {group.Key}");
    foreach (var person in group)
        Console.WriteLine($"  {person.Name}");
}
```

Example: Joining Collections

```csharp
var students = new[]
{
    new { Id = 1, Name = "John" },
    new { Id = 2, Name = "Jane" },
};

var marks = new[]
{
    new { StudentId = 1, Score = 85 },
    new { StudentId = 2, Score = 92 },
};

var result = students.Join(
    marks,
    s => s.Id,
    m => m.StudentId,
    (s, m) => new { s.Name, m.Score });

foreach (var r in result)
    Console.WriteLine($"{r.Name} - {r.Score}");
```

💡 Equivalent to an SQL INNER JOIN

## ⚙️ 7. LINQ with Complex Types

```csharp
class Employee
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public string Department { get; set; } = "";
    public double Salary { get; set; }
}

List<Employee> employees = new()
{
    new() { Id = 1, Name = "Alice", Department = "IT", Salary = 50000 },
    new() { Id = 2, Name = "Bob", Department = "HR", Salary = 45000 },
    new() { Id = 3, Name = "Charlie", Department = "IT", Salary = 60000 },
    new() { Id = 4, Name = "Diana", Department = "Finance", Salary = 55000 }
};

// Employees in IT earning > 50,000
var highPaidIT = employees
    .Where(e => e.Department == "IT" && e.Salary > 50000)
    .Select(e => new { e.Name, e.Salary });

foreach (var e in highPaidIT)
    Console.WriteLine($"{e.Name} - {e.Salary}");
```

## ⚙️ 8. Aggregation

```csharp
var salaries = employees.Select(e => e.Salary);

Console.WriteLine($"Total Salary: {salaries.Sum()}");
Console.WriteLine($"Average Salary: {salaries.Average()}");
Console.WriteLine($"Highest Salary: {salaries.Max()}");
```

## ⚙️ 9. Deferred Execution

LINQ queries are not executed immediately — they are deferred until iterated.

```csharp
var query = employees.Where(e => e.Salary > 50000);

// Adding new employee after query defined
employees.Add(new Employee { Id = 5, Name = "Eve", Department = "IT", Salary = 70000 });

// Query executes here (foreach)
foreach (var e in query)
    Console.WriteLine(e.Name);
```

💡 Output includes Eve because LINQ executes lazily.

## ⚙️ 10. Immediate Execution

Use methods like .ToList(), .ToArray(), .Count() to force execution immediately.

```csharp
var result = employees.Where(e => e.Salary > 50000).ToList();
```