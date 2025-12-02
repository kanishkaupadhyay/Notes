# 🗓️ Day 10 – Delegates, Func, Action, and Predicate in C#

## 🎯 Goal for Today
Understand **delegates** — how they encapsulate methods, enable callbacks, and serve as the foundation for **events**, **LINQ**, and **async programming**.  
Learn the built-in delegate types: `Func`, `Action`, and `Predicate`.

---

## ⚙️ 1. What is a Delegate?

A **delegate** is a **type-safe function pointer** — it holds a reference to a method and allows you to call that method indirectly.

Think of it as a variable that stores a function.

```csharp
delegate void GreetDelegate(string name); // Declaration

class Program
{
    static void SayHello(string name) => Console.WriteLine($"Hello, {name}");
    static void SayBye(string name) => Console.WriteLine($"Goodbye, {name}");

    static void Main()
    {
        GreetDelegate greet = SayHello; // Assign method
        greet("Alice");                  // Invoke

        greet = SayBye;
        greet("Alice");
    }
}
```

### 🧠 Output:

```csharp
Hello, Alice
Goodbye, Alice
```

## ⚙️ 2. Delegate Declaration and Invocation

```csharp
delegate int MathOperation(int a, int b);

class Program
{
    static int Add(int x, int y) => x + y;
    static int Multiply(int x, int y) => x * y;

    static void Main()
    {
        MathOperation operation = Add;
        Console.WriteLine(operation(3, 5)); // 8

        operation = Multiply;
        Console.WriteLine(operation(3, 5)); // 15
    }
}
```
💡 Delegates ensure compile-time type safety — you can only assign methods that match their signature.

## ⚙️ 3. Multicast Delegates

A delegate can reference multiple methods.
When invoked, all methods are called in order.

```csharp
delegate void Notify();

class Program
{
    static void SendEmail() => Console.WriteLine("Email sent!");
    static void SendSMS() => Console.WriteLine("SMS sent!");

    static void Main()
    {
        Notify notify = SendEmail;
        notify += SendSMS;

        notify(); // Calls both
    }
}
```
🧠 Output:

```csharp
Email sent!
SMS sent!
```
💡 If the delegate returns a value, only the last method’s return value is used.

## ⚙️ 4. Anonymous Methods

You can create a delegate without a named method using the delegate keyword.

delegate void ShowMessage(string msg);

```csharp
class Program
{
    static void Main()
    {
        ShowMessage message = delegate (string msg)
        {
            Console.WriteLine(msg);
        };

        message("Hello from anonymous method!");
    }
}
```
💡 Great for short-lived logic (used heavily before lambdas were introduced).

## ⚙️ 6. Built-in Delegates: Func, Action, Predicate

To avoid declaring custom delegates for every case, .NET provides generic delegates in the System namespace.

### 🔹 Action Delegate

Used for methods that return void.

```csharp
Action<string> greet = name => Console.WriteLine($"Hello, {name}");
greet("Kanishka");
```

You can also use multiple parameters:

```csharp
Action<int, int> add = (a, b) => Console.WriteLine(a + b);
add(5, 10);
```

🧠 Syntax:
Action<T1, T2, ..., T16> — up to 16 parameters.

### 🔹 Func Delegate

Used for methods that return a value.

```csharp
Func<int, int, int> multiply = (a, b) => a * b;
Console.WriteLine(multiply(4, 5)); // 20
```

🧠 Syntax:
Func<T1, T2, ..., TResult> — last type is the return type.

Example with transformation:

```csharp
Func<string, int> getLength = str => str.Length;
Console.WriteLine(getLength("Hello")); // 5
```

### 🔹 Predicate Delegate

Used for Boolean-returning conditions.

```csharp
Predicate<int> isEven = n => n % 2 == 0;
Console.WriteLine(isEven(4)); // True
Console.WriteLine(isEven(5)); // False
```
💡 Internally, Predicate<T> is just a Func<T, bool>.

## ⚙️ 7. Real-Life Example: Filtering Data with Delegates

```csharp
List<int> numbers = new() { 1, 2, 3, 4, 5, 6 };

List<int> Filter(List<int> source, Predicate<int> condition)
{
    List<int> result = new();
    foreach (var n in source)
    {
        if (condition(n))
            result.Add(n);
    }
    return result;
}

var evens = Filter(numbers, n => n % 2 == 0);
evens.ForEach(Console.WriteLine); // 2, 4, 6
```
💡 This demonstrates how delegates enable flexible, reusable filtering logic — similar to how LINQ Where() works internally.

## ⚙️ 8. Delegates and Events

Delegates form the foundation for events in C# (you’ll use this in ASP.NET Web API and GUI frameworks).

```csharp
public delegate void ProcessCompleted();

public class Process
{
    public event ProcessCompleted OnCompleted;

    public void Start()
    {
        Console.WriteLine("Process started...");
        Thread.Sleep(1000);
        OnCompleted?.Invoke();
    }
}

class Program
{
    static void Main()
    {
        Process process = new();
        process.OnCompleted += () => Console.WriteLine("Process finished!");
        process.Start();
    }
}
```

🧠 Here, event ensures controlled access to delegate invocation — publisher-subscriber pattern in action.

## ⚙️ 9. Chaining and Passing Delegates

Delegates can be passed as parameters — allowing callbacks.

```csharp
void PerformAction(int a, int b, Func<int, int, int> operation)
{
    Console.WriteLine("Result: " + operation(a, b));
}

PerformAction(5, 3, (x, y) => x + y); // Result: 8
PerformAction(5, 3, (x, y) => x * y); // Result: 15
```
💡 Used heavily in LINQ, EF, ASP.NET middleware pipelines, etc.

## ⚙️ 10. Summary Table

| Type               | Signature         | Returns | Use Case                    |
| ------------------ | ----------------- | ------- | --------------------------- |
| `delegate`         | Custom definition | Any     | Legacy / custom logic       |
| `Action<T>`        | `(T) => void`     | void    | When no return value needed |
| `Func<T, TResult>` | `(T) => TResult`  | Any     | When return value needed    |
| `Predicate<T>`     | `(T) => bool`     | bool    | For true/false conditions   |
| `event`            | Based on delegate | void    | Notifications, Pub/Sub      |
