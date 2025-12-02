# 🧠 Day 23 – Logging in ASP.NET Core

---

## 🎯 Goal for Today

By the end of this lesson, you’ll understand:

✅ Why structured logging is critical in real-world APIs  
✅ How to use ASP.NET Core’s **built-in logging system**  
✅ How to write logs to **console, files, and databases**  
✅ Logging best practices (structured logs, levels, correlation IDs)

---

## 🧩 1. Why Logging Matters

Logging is essential for:
- 📊 Monitoring application health
- 🪲 Debugging and error tracing
- 🔍 Audit and compliance
- 🧠 Understanding user behavior

ASP.NET Core provides a **built-in logging abstraction** through the `ILogger<T>` interface — and tools like **Serilog** or **NLog** extend it for advanced logging.

---

## ⚙️ 2. Built-In Logging in ASP.NET Core

### Example

```csharp
public class ProductService
{
    private readonly ILogger<ProductService> _logger;

    public ProductService(ILogger<ProductService> logger)
    {
        _logger = logger;
    }

    public void AddProduct(Product product)
    {
        _logger.LogInformation("Adding product: {ProductName}", product.Name);

        if (product.Price <= 0)
        {
            _logger.LogWarning("Product {ProductName} has invalid price: {Price}", product.Name, product.Price);
        }

        try
        {
            throw new InvalidOperationException("Database error");
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to add product {ProductName}", product.Name);
        }
    }
}
```

✅ Logging levels:

- LogTrace() → detailed diagnostics
- LogDebug() → for developers
- LogInformation() → general flow
- LogWarning() → recoverable issues
- LogError() → errors in operations
- LogCritical() → application failures

✅ Use appropriate log levels:

| Level       | Meaning                             |
| ----------- | ----------------------------------- |
| Information | Business flow                       |
| Warning     | Unexpected but recoverable          |
| Error       | Failed but handled                  |
| Critical    | System crash or unrecoverable error |

🧭 Summary

| Concept                | Description                                      |
| ---------------------- | ------------------------------------------------ |
| **ILogger<T>**         | Built-in logging abstraction                     |
| **Serilog**            | Structured, extensible logging                   |
| **NLog**               | XML-configurable logging framework               |
| **Sinks / Targets**    | Where logs are written (file, console, DB, etc.) |
| **Structured Logging** | Machine-readable data for search/filtering       |
| **Best Practice**      | Log meaningful, context-rich data                |