# 🗓️ Day 13 – File Handling & JSON Serialization in C#

## 🎯 Goal for Today
Learn how to read and write files, and serialize/deserialize objects to JSON using `System.Text.Json` — essential for data storage and APIs.

---

## ⚙️ 1. Introduction

- **File Handling** allows us to read/write data to files on disk.  
- **Serialization** converts objects → text (e.g., JSON/XML).  
- **Deserialization** converts text → objects.

Used for:
- Configuration files  
- Saving application data  
- API communication (JSON)  
- Logging  

---

## ⚙️ 2. File Handling in C#

Namespace: `System.IO`

### 🔹 Writing to a Text File

```csharp
using System.IO;

string path = "log.txt";
string message = "Hello, this is a log message.";

File.WriteAllText(path, message);
Console.WriteLine("File written successfully!");
```

✅ If file doesn’t exist, it’s created automatically.

### 🔹 Appending Data

```csharp
File.AppendAllText("log.txt", "\nAnother log entry at " + DateTime.Now);
```

### 🔹 Reading a File

```csharp
string content = File.ReadAllText("log.txt");
Console.WriteLine(content);
```

### 🔹 Reading Line by Line

```csharp
string[] lines = File.ReadAllLines("log.txt");
foreach (var line in lines)
{
    Console.WriteLine(line);
}
```

### 🔹 Using StreamWriter and StreamReader

When working with large files, prefer streams.

```csharp
using (StreamWriter writer = new StreamWriter("data.txt"))
{
    writer.WriteLine("Line 1");
    writer.WriteLine("Line 2");
}

using (StreamReader reader = new StreamReader("data.txt"))
{
    string line;
    while ((line = reader.ReadLine()) != null)
    {
        Console.WriteLine(line);
    }
}
```

## ⚙️ 3. JSON Serialization

Namespace: System.Text.Json

Introduced in .NET Core 3.0, it’s the recommended JSON library now (faster than Newtonsoft.Json).

### 🔹 Serializing an Object

```csharp
using System.Text.Json;

var person = new
{
    Name = "Alice",
    Age = 25,
    Email = "alice@example.com"
};

string json = JsonSerializer.Serialize(person);
Console.WriteLine(json);
```

🧾 Output:

```json
{"Name":"Alice","Age":25,"Email":"alice@example.com"}
```

### 🔹 Pretty Printing (Indented JSON)

```csharp
var options = new JsonSerializerOptions { WriteIndented = true };
string formattedJson = JsonSerializer.Serialize(person, options);
Console.WriteLine(formattedJson);
```

### 🔹 Serializing Custom Class

```csharp
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Department { get; set; }
}

var emp = new Employee { Id = 1, Name = "John", Department = "IT" };
string json = JsonSerializer.Serialize(emp);
Console.WriteLine(json);
```

## ⚙️ 4. JSON Deserialization

Convert JSON → Object.

```csharp
string json = "{\"Id\":1,\"Name\":\"John\",\"Department\":\"IT\"}";
Employee emp = JsonSerializer.Deserialize<Employee>(json);
Console.WriteLine($"Name: {emp.Name}, Dept: {emp.Department}");
```

### 🔹 Deserialize a List

```csharp
string jsonList = "[{\"Id\":1,\"Name\":\"A\"},{\"Id\":2,\"Name\":\"B\"}]";
List<Employee> employees = JsonSerializer.Deserialize<List<Employee>>(jsonList);

foreach (var e in employees)
    Console.WriteLine(e.Name);
```

## ⚙️ 5. JSON File Handling Example

Combine file + JSON.

```csharp
using System.Text.Json;

var employees = new List<Employee>
{
    new Employee { Id = 1, Name = "John", Department = "HR" },
    new Employee { Id = 2, Name = "Sara", Department = "Finance" }
};

// Serialize to JSON and write to file
string json = JsonSerializer.Serialize(employees, new JsonSerializerOptions { WriteIndented = true });
File.WriteAllText("employees.json", json);

// Read from file and deserialize
string readJson = File.ReadAllText("employees.json");
var result = JsonSerializer.Deserialize<List<Employee>>(readJson);

foreach (var emp in result)
    Console.WriteLine($"{emp.Id} - {emp.Name} ({emp.Department})");
```
✅ You just created a simple persistent data store!

## ⚙️ 6. Customizing JSON Serialization

### 🔹 Ignoring Properties

```csharp
public class Product
{
    public int Id { get; set; }

    [JsonIgnore]
    public decimal CostPrice { get; set; }

    public decimal SellingPrice { get; set; }
}
```

### 🔹 Custom Property Names

```csharp
public class User
{
    [JsonPropertyName("user_name")]
    public string Name { get; set; }
}
```

Output:

```json
{"user_name": "Alice"}
```

## ⚙️ 7. Handling JSON in ASP.NET Core APIs

ASP.NET Core uses System.Text.Json by default for:

Request body → Object ([FromBody])

Object → JSON response

Example:

```csharp
[HttpPost]
public IActionResult SaveEmployee([FromBody] Employee employee)
{
    return Ok(new { message = "Employee received", employee });
}
```

Client sends JSON:

```json
{
  "id": 1,
  "name": "John",
  "department": "Finance"
}
```
✅ ASP.NET Core automatically deserializes and serializes for you.

## ⚙️ 8. Advanced JSON Options

You can configure global JSON behavior in ASP.NET Core:

```csharp
builder.Services.AddControllers().AddJsonOptions(options =>
{
    options.JsonSerializerOptions.PropertyNamingPolicy = JsonNamingPolicy.CamelCase;
    options.JsonSerializerOptions.WriteIndented = true;
});
```
This ensures camelCase naming in all responses.

## ⚙️ 9. Error Handling in File & JSON Operations

Always handle exceptions for IO operations.

```csharp
try
{
    string content = File.ReadAllText("missing.txt");
}
catch (FileNotFoundException ex)
{
    Console.WriteLine("File not found!");
}
catch (JsonException ex)
{
    Console.WriteLine("Invalid JSON format!");
}
```

✅ Summary

| Concept                      | Description                                           |
| ---------------------------- | ----------------------------------------------------- |
| **File Handling**            | Read/Write files using `File` or streams              |
| **Serialization**            | Object → JSON using `JsonSerializer.Serialize()`      |
| **Deserialization**          | JSON → Object using `JsonSerializer.Deserialize<T>()` |
| **Attributes**               | `[JsonIgnore]`, `[JsonPropertyName]`                  |
| **ASP.NET Core Integration** | JSON handled automatically via model binding          |
| **Exception Handling**       | Always use try-catch around IO/JSON operations        |
