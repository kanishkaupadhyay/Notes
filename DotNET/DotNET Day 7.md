# 🗓️ Day 7 – Exception Handling, File I/O, and Debugging in C#

## 🎯 Goal for Today
Learn how to handle runtime errors gracefully, work with files, and use debugging techniques in C# for advanced, robust applications.

---

## ⚙️ 1. Exception Handling

Exceptions are **runtime errors** that can occur during program execution. Proper handling ensures your program doesn't crash unexpectedly.

### 🔹 Try-Catch-Finally Block

```csharp
try
{
    int a = 10;
    int b = 0;
    int result = a / b; // Division by zero!
}
catch (DivideByZeroException ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}
finally
{
    Console.WriteLine("This block always executes.");
}
```

✅ Notes:

try → Code that may throw an exception

catch → Handles specific exceptions

finally → Executes always, used for cleanup

### 🔹 Multiple Catch Blocks

```csharp
try
{
    string text = null;
    Console.WriteLine(text.Length);
}
catch (DivideByZeroException ex)
{
    Console.WriteLine("Cannot divide by zero.");
}
catch (NullReferenceException ex)
{
    Console.WriteLine("Object is null.");
}
catch (Exception ex)
{
    Console.WriteLine($"General error: {ex.Message}");
}
```

### 🔹 Throwing Exceptions

You can throw exceptions manually:

```csharp
class BankAccount
{
    private double balance;

    public void Withdraw(double amount)
    {
        if (amount > balance)
            throw new InvalidOperationException("Insufficient funds!");
        balance -= amount;
    }
}
```

### 🔹 Best Practices

Catch specific exceptions rather than general Exception.

Always release resources (use finally or using).

Avoid empty catch blocks.

Prefer try-catch around critical code only.

## ⚙️ 2. File I/O in C#

C# provides classes in System.IO to read from and write to files.

### 🔹 Reading Text Files

```csharp
using System.IO;

string path = "example.txt";

// Read all text
string content = File.ReadAllText(path);
Console.WriteLine(content);

// Read lines
string[] lines = File.ReadAllLines(path);
foreach (var line in lines)
{
    Console.WriteLine(line);
}
```

### 🔹 Writing to Text Files

```csharp
string[] data = { "Line 1", "Line 2", "Line 3" };
File.WriteAllLines("output.txt", data);

// Append a line
File.AppendAllText("output.txt", "\nLine 4");
```

### 🔹 Using StreamReader and StreamWriter

```csharp
// Reading
using (StreamReader reader = new StreamReader("example.txt"))
{
    string line;
    while ((line = reader.ReadLine()) != null)
    {
        Console.WriteLine(line);
    }
}

// Writing
using (StreamWriter writer = new StreamWriter("output.txt"))
{
    writer.WriteLine("Hello World!");
    writer.WriteLine("Writing with StreamWriter");
}
```
💡 Tip: using automatically disposes the resource, avoiding memory leaks.

### 🔹 Binary File I/O (Optional)

```csharp
using (FileStream fs = new FileStream("data.bin", FileMode.Create))
{
    byte[] bytes = { 1, 2, 3, 4, 5 };
    fs.Write(bytes, 0, bytes.Length);
}
```

## ⚙️ 3. Debugging in C#

Debugging is crucial to identify, trace, and fix errors.

### 🔹 Techniques

1. Breakpoints: Stop execution at a specific line.

2. Step Over (F10): Execute the current line and move to the next.

3. Step Into (F11): Go inside the method call.

4. Step Out (Shift+F11): Finish current method and return.

5. Watch/Immediate Window: Evaluate expressions and variables at runtime.

6. Exception Settings: Configure Visual Studio to break when exceptions are thrown.