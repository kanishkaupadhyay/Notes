# 🗓️ Day 12 – Dependency Injection (DI) Fundamentals

## 🎯 Goal for Today
Learn what **Dependency Injection** is, why it’s so powerful, and how to implement it in C# and ASP.NET Core.

---

## ⚙️ 1. What is Dependency Injection?

**Dependency Injection (DI)** is a design pattern used to achieve **Inversion of Control (IoC)** between classes and their dependencies.

Instead of creating objects directly inside a class, dependencies are **provided (injected)** from outside.

---

### 💡 Without DI (Tight Coupling)

```csharp
public class EmailService
{
    public void SendEmail(string message)
    {
        Console.WriteLine($"Email sent: {message}");
    }
}

public class NotificationManager
{
    private EmailService _emailService = new EmailService(); // ❌ Tight coupling

    public void Notify(string message)
    {
        _emailService.SendEmail(message);
    }
}
```
Here, NotificationManager depends directly on EmailService.
You cannot easily replace or test it.

✅ With DI (Loose Coupling)

## ⚙️ 2. Benefits of DI

| Benefit             | Description                                                   |
| ------------------- | ------------------------------------------------------------- |
| **Loose Coupling**  | Classes depend on abstractions, not concrete implementations. |
| **Testability**     | Easy to mock dependencies in unit tests.                      |
| **Maintainability** | Change implementations without touching consumers.            |
| **Flexibility**     | Different implementations can be injected dynamically.        |


## ⚙️ 3. Types of Dependency Injection

| Type                      | Example               | Description                    |
| ------------------------- | --------------------- | ------------------------------ |
| **Constructor Injection** | via class constructor | Most common and recommended    |
| **Property Injection**    | via public property   | Useful when optional           |
| **Method Injection**      | via method parameters | Used for one-time dependencies |


🧩 Constructor Injection (Best Practice)

```csharp
public class ReportGenerator
{
    private readonly ILogger _logger;

    public ReportGenerator(ILogger logger)
    {
        _logger = logger;
    }

    public void Generate()
    {
        _logger.Log("Report generated!");
    }
}
```

## ⚙️ 4. DI in ASP.NET Core

ASP.NET Core comes with a built-in IoC container.

You register services in Program.cs or Startup.cs, and the framework automatically injects them where needed.

🧠 Example: Registering Services

```csharp
// Program.cs
using Microsoft.Extensions.DependencyInjection;

var builder = WebApplication.CreateBuilder(args);

// Register services
builder.Services.AddScoped<IEmailService, EmailService>();
builder.Services.AddScoped<NotificationManager>();

var app = builder.Build();

app.MapGet("/", (NotificationManager manager) =>
{
    manager.Notify("Hello from DI!");
    return "Notification sent!";
});

app.Run();
```

🧩 Explanation

| Code                                                                       | Description                       |
| -------------------------------------------------------------------------- | --------------------------------- |
| `AddScoped<IEmailService, EmailService>()`                                 | Registers the service.            |
| `AddScoped<NotificationManager>()`                                         | Register class with dependencies. |

Framework automatically injects dependencies when creating the object.

## ⚙️ 5. DI Lifetimes

| Lifetime      | Description                                    | Example            |
| ------------- | ---------------------------------------------- | ------------------ |
| **Transient** | New instance created every time it’s requested | `AddTransient<>()` |
| **Scoped**    | One instance per web request                   | `AddScoped<>()`    |
| **Singleton** | Single instance for the entire app lifetime    | `AddSingleton<>()` |

Example:

```csharp
builder.Services.AddTransient<ITransientService, TransientService>();
builder.Services.AddScoped<IScopedService, ScopedService>();
builder.Services.AddSingleton<ISingletonService, SingletonService>();
```

💡 Use Scoped for database-related services (like repositories).

## ⚙️ 6. Example – Repository and Service Layer

Let’s see DI in action in a real-world scenario 👇

```csharp
// IProductRepository.cs
public interface IProductRepository
{
    Task<IEnumerable<string>> GetProductsAsync();
}

// ProductRepository.cs
public class ProductRepository : IProductRepository
{
    public async Task<IEnumerable<string>> GetProductsAsync()
    {
        await Task.Delay(500);
        return new[] { "Laptop", "Mouse", "Keyboard" };
    }
}

// ProductService.cs
public interface IProductService
{
    Task<IEnumerable<string>> GetProductsAsync();
}

public class ProductService : IProductService
{
    private readonly IProductRepository _repository;

    public ProductService(IProductRepository repository)
    {
        _repository = repository;
    }

    public async Task<IEnumerable<string>> GetProductsAsync()
    {
        return await _repository.GetProductsAsync();
    }
}

// ProductController.cs
[ApiController]
[Route("api/[controller]")]
public class ProductController : ControllerBase
{
    private readonly IProductService _productService;

    public ProductController(IProductService productService)
    {
        _productService = productService;
    }

    [HttpGet]
    public async Task<IActionResult> Get()
    {
        var products = await _productService.GetProductsAsync();
        return Ok(products);
    }
}
```

In Program.cs

```csharp
builder.Services.AddScoped<IProductRepository, ProductRepository>();
builder.Services.AddScoped<IProductService, ProductService>();
```

✅ Now your controller, service, and repository are automatically connected by DI.

## ⚙️ 7. Injecting Built-in Services

ASP.NET Core includes many injectable services:

ILogger<T>

IConfiguration

IWebHostEnvironment

IHttpClientFactory

Example:

```csharp
public class LogDemo
{
    private readonly ILogger<LogDemo> _logger;

    public LogDemo(ILogger<LogDemo> logger)
    {
        _logger = logger;
    }

    public void DoWork()
    {
        _logger.LogInformation("Work started");
    }
}
```

## ⚙️ 8. Example – Multiple Implementations

```csharp
public interface IMessageService
{
    void Send(string msg);
}

public class EmailMessageService : IMessageService
{
    public void Send(string msg) => Console.WriteLine($"Email: {msg}");
}

public class SmsMessageService : IMessageService
{
    public void Send(string msg) => Console.WriteLine($"SMS: {msg}");
}
```

Register both:

```csharp
builder.Services.AddScoped<IMessageService, EmailMessageService>();
builder.Services.AddScoped<IMessageService, SmsMessageService>();
```

Inject as IEnumerable<IMessageService>:

```csharp
public class MessageProcessor
{
    private readonly IEnumerable<IMessageService> _messageServices;

    public MessageProcessor(IEnumerable<IMessageService> messageServices)
    {
        _messageServices = messageServices;
    }

    public void Process()
    {
        foreach (var service in _messageServices)
            service.Send("Hello!");
    }
}
```
✅ Both services are used in one go!

## ⚙️ 9. Mocking and Testing with DI

DI makes unit testing simple.

Example test:

```csharp
[Fact]
public async Task GetProducts_ReturnsCorrectCount()
{
    var mockRepo = new Mock<IProductRepository>();
    mockRepo.Setup(r => r.GetProductsAsync()).ReturnsAsync(new[] { "Laptop" });

    var service = new ProductService(mockRepo.Object);
    var result = await service.GetProductsAsync();

    Assert.Single(result);
}
```
💡 Because we injected an interface, we can replace it with a mock in tests!

✅ Summary

| Concept                                     | Description                             |
| ------------------------------------------- | --------------------------------------- |
| **Dependency Injection**                    | External object creation and management |
| **IoC**                                     | Object creation control is inverted     |
| **AddScoped / AddSingleton / AddTransient** | Control lifetime of dependencies        |
| **Constructor Injection**                   | Most common and cleanest                |
| **ASP.NET Core**                            | Has built-in DI container               |
| **Testing**                                 | DI makes mocking and unit testing easy  |