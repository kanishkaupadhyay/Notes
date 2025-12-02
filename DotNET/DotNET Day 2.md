# 🗓️ Day 2 – C# Syntax, Data Types, and Variables

## 🎯 Goal for Today
Understand the basic syntax of C#, the types of data you can use, and how variables work — the foundation for every .NET application.

---

## 🧩 1. C# Program Structure

A simple C# program looks like this:

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello, .NET World!");
        }
    }
}
```

### 🧠 Breakdown:
- `using System;` → Imports the **System namespace**
- `namespace` → Groups related classes logically
- `class Program` → Class declaration
- `static void Main()` → Entry point of the application
- `Console.WriteLine()` → Prints to the console

---

## 🧮 2. Data Types in C#

C# is **strongly typed**, meaning each variable must have a type.

### 🔹 Value Types
Stored in **stack memory**, contain actual data.

| Type | Example | Description |
|------|----------|-------------|
| `int` | 10 | Integer numbers |
| `double` | 10.5 | Floating-point numbers |
| `decimal` | 10.50M | High-precision decimal (used in finance) |
| `bool` | true / false | Boolean value |
| `char` | 'A' | Single character |
| `struct` | Custom value type | User-defined |

### 🔸 Reference Types
Stored in **heap memory**, contain a reference (address) to data.

| Type | Example | Description |
|------|----------|-------------|
| `string` | "Hello" | Sequence of characters |
| `object` | any type | Base type of all objects |
| `class` | custom class | User-defined reference type |
| `array` | new int[5] | Collection of elements |
| `interface` | custom interface | Defines a contract |

---

## 🧰 3. Declaring and Initializing Variables

```csharp
int age = 25;
string name = "Kanishka";
bool isActive = true;
double salary = 55000.75;
char grade = 'A';
```

You can also declare multiple variables:
```csharp
int a = 5, b = 10, c = 15;
```

Or use **type inference** with `var`:
```csharp
var city = "Chennai"; // Compiler infers this as string
var temperature = 32.5; // Inferred as double
```

---

## 🧩 4. Constants and Readonly

### `const` → Value must be known at **compile time**
```csharp
const double Pi = 3.14159;
```

### `readonly` → Can be set only **once**, typically in a constructor
```csharp
readonly string appName;
public Program()
{
    appName = "CaseFilePhysicalManagement";
}
```

---

## ⚙️ 5. Type Conversion

### Implicit Conversion (Safe)
```csharp
int num = 100;
double value = num; // Implicitly converted to double
```

### Explicit Conversion (Cast)
```csharp
double salary = 1234.56;
int truncated = (int)salary; // Explicit cast
```

### Using `Convert` or `Parse`
```csharp
string input = "25";
int age = int.Parse(input); // Throws if invalid
int safeAge = Convert.ToInt32(input); // Safer
```

---

## 🔍 6. Strings in C#

```csharp
string message = "Welcome to .NET " + "Training!";
string formatted = $"Hello, {name}. You are {age} years old.";
string multiLine = @"This is
a multi-line
string!";
```

---

## 💡 7. Nullables and `var` Keyword

C# supports **nullable types** using `?`:
```csharp
int? marks = null;
if (marks.HasValue)
    Console.WriteLine(marks.Value);
else
    Console.WriteLine("No value assigned");
```

---