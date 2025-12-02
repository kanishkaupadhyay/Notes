# 🗓️ Day 11 – Async / Await and Tasks in C#

## 🎯 Goal for Today
Understand **asynchronous programming**, how to use **Task**, **async/await**, and how they make your application **non-blocking** and **scalable**.

---

## ⚙️ 1. Why Async Programming?

Modern apps often perform operations that take time:
- Database calls
- File I/O
- Web requests
- Long computations

If done **synchronously**, these block the thread — making apps **slow or unresponsive**.

💡 **Async programming** allows you to start tasks, free up the thread, and continue when the work is done.

---

## ⚙️ 2. The Problem (Synchronous Code)

```csharp
public string DownloadData()
{
    Thread.Sleep(3000); // Simulate delay
    return "Data downloaded!";
}

Console.WriteLine("Starting...");
var data = DownloadData();
Console.WriteLine(data);
Console.WriteLine("Done!");
```

🧱 Output (3 seconds delay before anything appears):

Starting...
Data downloaded!
Done!

The main thread waits for the operation to complete.
Not scalable for web servers!

## ⚙️ 3. The Solution (Asynchronous Code)

```csharp
public async Task<string> DownloadDataAsync()
{
    await Task.Delay(3000); // Simulate delay
    return "Data downloaded asynchronously!";
}

Console.WriteLine("Starting...");
var task = DownloadDataAsync();
Console.WriteLine("Doing other work...");
var result = await task;
Console.WriteLine(result);
Console.WriteLine("Done!");
```

✅ Output:

Starting...
Doing other work...
Data downloaded asynchronously!
Done!

💡 await releases the thread back to the caller while waiting — no blocking.

## ⚙️ 4. Key Concepts

| Concept   | Description                                                   |
| --------- | ------------------------------------------------------------- |
| `Task`    | Represents an asynchronous operation that may return a result |
| `async`   | Marks a method as asynchronous (can use `await` inside)       |
| `await`   | Suspends method execution until the awaited task completes    |
| `Task<T>` | Represents an async operation returning a result              |
| `void`    | Avoid in async except for event handlers (`async void`)       |

## ⚙️ 5. Anatomy of an Async Method

public async Task<int> CalculateAsync()
{
    await Task.Delay(2000);
    return 42;
}

🔍 Breakdown
1. async enables await inside method.
2. Task<int> means result will be an integer.
3. Method returns immediately, and resumes when awaited operation finishes.

## ⚙️ 6. Task vs Task<T> vs void

| Type      | Description                          | Example                        |
| --------- | ------------------------------------ | ------------------------------ |
| `Task`    | Runs asynchronously, returns nothing | `await SomeTask()`             |
| `Task<T>` | Runs asynchronously, returns a value | `await GetDataAsync()`         |
| `void`    | Only for event handlers              | `async void Button_Click(...)` |

## ⚙️ 7. Parallel Async Tasks

You can run multiple async operations in parallel.

```csharp
async Task<string> GetDataAsync(string source)
{
    await Task.Delay(2000);
    return $"Data from {source}";
}

var task1 = GetDataAsync("Server 1");
var task2 = GetDataAsync("Server 2");

var results = await Task.WhenAll(task1, task2);

foreach (var r in results)
    Console.WriteLine(r);
```

## ⚙️ 8. Fire-and-Forget Tasks (Careful!)

```csharp
Task.Run(() => Console.WriteLine("Running in background"));
```

💡 Used for background work, but exceptions may be lost.
Prefer proper awaiting or error handling.

## ⚙️ 9. Exception Handling in Async

Always wrap await calls in try/catch.
```csharp
try
{
    await Task.Run(() => throw new Exception("Something went wrong"));
}
catch (Exception ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}
```
💡 Exception rethrows automatically inside await.

## ⚙️ 10. Real-World Example - Async File Reading

```csharp
using System.IO;

async Task ReadFileAsync()
{
    using StreamReader reader = new("data.txt");
    string content = await reader.ReadToEndAsync();
    Console.WriteLine(content);
}

await ReadFileAsync();
```
✅ Non-blocking file I/O.

## ⚙️ 11. Async in ASP.NET Core Web API
Example: Fetching data from service

```csharp
[HttpGet("products")]
public async Task<IActionResult> GetProducts()
{
    var products = await _productService.GetAllProductsAsync();
    return Ok(products);
}
```
💡 Controllers should always use async/await to keep request threads free.

## ⚙️ 12. CPU-bound vs I/O-bound
| Type      | Example                            | Use                                           |
| --------- | ---------------------------------- | --------------------------------------------- |
| CPU-bound | Heavy calculation, data processing | Use `Task.Run()` to move to background thread |
| I/O-bound | File, DB, network                  | Use `await` with async I/O methods            |

## ⚙️ 14. Parallel Async Example (Web Calls Simulation)

```csharp
async Task<string> FetchDataAsync(string url)
{
    await Task.Delay(1000);
    return $"Data from {url}";
}

var urls = new[] { "api1", "api2", "api3" };
var tasks = urls.Select(u => FetchDataAsync(u));
var results = await Task.WhenAll(tasks);

foreach (var res in results)
    Console.WriteLine(res);
```
✅ All 3 API calls happen simultaneously — total ≈ 1 second.

## ⚙️ 15. Best Practices

✅ Always return Task or Task<T>, not void.
✅ Use ConfigureAwait(false) in libraries.
✅ Avoid .Result or .Wait() (they block).
✅ Combine async calls with Task.WhenAll() where possible.
✅ Use async all the way down (from controller → repository).