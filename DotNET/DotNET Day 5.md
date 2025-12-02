# 🗓️ Day 5 – Object-Oriented Programming (OOP) in C#

## 🎯 Goal for Today
Understand the fundamentals of **OOP** in C# — creating and using classes, objects, and methods to structure your programs efficiently.

---

## ⚙️ 1. Classes and Objects

### 🔹 What is a Class?
A **class** is a blueprint for creating objects. It defines **properties** (data) and **methods** (behavior).

```csharp
class Student
{
    public int Id;
    public string Name;

    public void Display()
    {
        Console.WriteLine($"ID: {Id}, Name: {Name}");
    }
}
```

### 🔹 Creating an Object

```csharp
Student student1 = new Student();
student1.Id = 101;
student1.Name = "Kanishka";

student1.Display(); // Output: ID: 101, Name: Kanishka
```

## 🧩 2. Properties in C#

Properties provide a controlled way to access fields.

```csharp
class Student
{
    private int id;

    public int Id
    {
        get { return id; }
        set { id = value; }
    }

    public string Name { get; set; } // Auto-implemented property
}
```

🔹 Using Properties

```csharp
Student s = new Student();
s.Id = 102;
s.Name = "Mary";
Console.WriteLine($"ID: {s.Id}, Name: {s.Name}");
```

## ⚙️ 3. Methods

Methods define behavior for a class.

```csharp
class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public int Multiply(int a, int b) => a * b; // Expression-bodied method
}
```

🔹 Calling Methods

```csharp
Calculator calc = new Calculator();
Console.WriteLine(calc.Add(5, 10));      // Output: 15
Console.WriteLine(calc.Multiply(4, 3));  // Output: 12
```

## 🧩 4. Constructors

A constructor initializes an object when created.

```csharp
class Student
{
    public int Id;
    public string Name;

    // Constructor
    public Student(int id, string name)
    {
        Id = id;
        Name = name;
    }

    public void Display() => Console.WriteLine($"ID: {Id}, Name: {Name}");
}

// Usage
Student s1 = new Student(101, "Kanishka");
s1.Display(); // Output: ID: 101, Name: Kanishka
```

## ⚙️ 5. Access Modifiers

| Modifier             | Access Level                             |
| -------------------- | ---------------------------------------- |
| `public`             | Accessible anywhere                      |
| `private`            | Accessible only within class             |
| `protected`          | Accessible within class & derived        |
| `internal`           | Accessible within assembly               |
| `protected internal` | Accessible in assembly + derived classes |

## 🧩 6. Static Members

Static members belong to the class, not the instance.

```csharp
class MathHelper
{
    public static double Pi = 3.14159;

    public static double Square(double n)
    {
        return n * n;
    }
}

// Usage
Console.WriteLine(MathHelper.Pi);       // 3.14159
Console.WriteLine(MathHelper.Square(5)); // 25
```

## ⚙️ 7. Inheritance (Preview)

Inheritance allows a class to reuse functionality from another class.

```csharp
class Person
{
    public string Name { get; set; }
}

class Employee : Person
{
    public int EmployeeId { get; set; }
}

//usage
Employee emp = new Employee();
emp.Name = "Mary";
emp.EmployeeId = 101;
Console.WriteLine($"Name: {emp.Name}, ID: {emp.EmployeeId}");
```