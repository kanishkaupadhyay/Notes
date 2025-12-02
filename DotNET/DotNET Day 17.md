# 🗓️ Day 17 – Model Binding and Validation in ASP.NET Core

## 🎯 Goal for Today
Learn how ASP.NET Core:
- Converts HTTP request data into C# objects (**Model Binding**)
- Ensures data correctness using validation attributes (**Model Validation**)
- Returns meaningful error messages when validation fails

---

## 🧠 1. What is Model Binding?

Model Binding is ASP.NET Core’s process of **automatically mapping incoming request data** (like JSON, query strings, or route parameters) into C# objects.

### Example

Request:
```http
POST /api/employees
Content-Type: application/json

{
  "id": 1,
  "name": "Alice",
  "department": "HR"
}
```

Controller:

```csharp
[HttpPost]
public IActionResult AddEmployee([FromBody] Employee employee)
{
    return Ok(employee.Name);
}
```
✅ ASP.NET Core automatically:

1. Reads the JSON body
2. Converts it to an Employee object
3. Passes it to your controller method

## 🧩 2. Sources of Data for Model Binding

| Source       | Attribute      | Example                    |
| ------------ | -------------- | -------------------------- |
| JSON Body    | `[FromBody]`   | `POST` requests            |
| URL Route    | `[FromRoute]`  | `/api/employee/2`          |
| Query String | `[FromQuery]`  | `/api/employee?name=Alice` |
| Form Data    | `[FromForm]`   | HTML forms                 |
| Header       | `[FromHeader]` | Custom headers             |

Example:

```csharp
[HttpGet("search")]
public IActionResult SearchEmployee([FromQuery] string name)
{
    return Ok($"Searching employee: {name}");
}
```

## ⚙️ 3. Example: Binding a Complex Object

Model:

```csharp
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public string Department { get; set; } = string.Empty;
}
```

Controller:

```csharp
[HttpPost]
public IActionResult Create([FromBody] Employee employee)
{
    return Ok($"Employee {employee.Name} created in {employee.Department}");
}
```

Request:
```json
{
  "id": 5,
  "name": "Kanishka",
  "department": "IT"
}
```

Response:

Employee Kanishka created in IT

## 🧠 4. What is Model Validation?

Model Validation ensures that incoming data is correct before your controller processes it.

It uses data annotation attributes placed on model properties.

Example

```csharp
public class Employee
{
    [Required(ErrorMessage = "Name is required")]
    [StringLength(50, MinimumLength = 2)]
    public string Name { get; set; } = string.Empty;

    [Range(18, 60, ErrorMessage = "Age must be between 18 and 60")]
    public int Age { get; set; }

    [Required]
    public string Department { get; set; } = string.Empty;
}
```

## ⚙️ 5. Handling Validation in Controller

When using [ApiController] (as we’ve been doing), ASP.NET Core automatically validates incoming models.

If validation fails:

1. It does not call your method
2. It automatically returns 400 Bad Request with error details

Example:

```csharp
[HttpPost]
public IActionResult AddEmployee([FromBody] Employee employee)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState); // Optional: not needed with [ApiController]

    return Ok(employee);
}
```

Example Request (Invalid)
```json
{
  "age": 15,
  "department": ""
}
```

Example Response

```json
{
  "errors": {
    "Name": ["Name is required"],
    "Age": ["Age must be between 18 and 60"],
    "Department": ["The Department field is required."]
  }
}
```

## ⚙️ 6. Common Validation Attributes

| Attribute             | Description                  | Example                                   |
| --------------------- | ---------------------------- | ----------------------------------------- |
| `[Required]`          | Field must not be null/empty | `[Required]`                              |
| `[StringLength]`      | Limits string length         | `[StringLength(100, MinimumLength = 3)]`  |
| `[Range]`             | Specifies numeric range      | `[Range(1, 100)]`                         |
| `[EmailAddress]`      | Valid email format           | `[EmailAddress]`                          |
| `[Phone]`             | Valid phone format           | `[Phone]`                                 |
| `[RegularExpression]` | Custom regex validation      | `[RegularExpression(@"^[A-Z]{2}\d{4}$")]` |
| `[Compare]`           | Compares two fields          | `[Compare("Password")]`                   |

## 🧩 7. Example: Custom Validation Attribute

You can also create your own validation rules.

```csharp
public class NotEmptyOrWhiteSpaceAttribute : ValidationAttribute
{
    public override bool IsValid(object? value)
    {
        return value is string str && !string.IsNullOrWhiteSpace(str);
    }
}
```

Use it:

```csharp
public class Department
{
    [NotEmptyOrWhiteSpace(ErrorMessage = "Department name cannot be empty.")]
    public string DepartmentName { get; set; } = string.Empty;
}
```

## ⚙️ 8. Manual Model Validation (Without [ApiController])

If [ApiController] is not used, you must manually check ModelState.

```csharp
[HttpPost]
public IActionResult AddEmployee([FromBody] Employee employee)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }
    return Ok(employee);
}
```

## 🧩 9. Nested Models

Validation also works for nested models.

Example:

```csharp
public class Address
{
    [Required]
    public string City { get; set; } = string.Empty;
}

public class Employee
{
    [Required]
    public string Name { get; set; } = string.Empty;

    [Required]
    public Address? Address { get; set; }
}
```
If Address is missing or invalid, ASP.NET Core will return a 400 Bad Request.

## ⚙️ 10. Model Binding from Multiple Sources

You can mix different [From*] attributes:

```csharp
[HttpPost("{id}")]
public IActionResult Update(
    [FromRoute] int id,
    [FromBody] Employee employee,
    [FromHeader(Name = "X-Client-Id")] string clientId)
{
    return Ok($"Updated employee {id} for client {clientId}");
}
```

## 🧩 11. Example: Full Controller with Validation

```csharp
[ApiController]
[Route("api/[controller]")]
public class EmployeesController : ControllerBase
{
    private static readonly List<Employee> Employees = new();

    [HttpPost]
    public IActionResult Create([FromBody] Employee employee)
    {
        if (!ModelState.IsValid)
            return BadRequest(ModelState);

        employee.Id = Employees.Count + 1;
        Employees.Add(employee);
        return CreatedAtAction(nameof(GetById), new { id = employee.Id }, employee);
    }

    [HttpGet("{id}")]
    public IActionResult GetById(int id)
    {
        var emp = Employees.FirstOrDefault(e => e.Id == id);
        return emp is null ? NotFound() : Ok(emp);
    }
}
```