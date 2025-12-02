# 🗓️ Day 15 – What is ASP.NET Core and Middleware

## 🎯 Goal for Today
Understand **what ASP.NET Core** is, its architecture, and how the **middleware pipeline** works — the backbone of every Web API request.

---

## 🧠 1. What is ASP.NET Core?

**ASP.NET Core** is a **cross-platform, open-source framework** for building:
- Web APIs
- Web apps (MVC / Razor)
- Real-time apps (SignalR)
- Microservices

It’s **modular, fast, and unified**, running on **Windows, Linux, and macOS**.

### 💡 Key Benefits:
- ✅ Cross-platform (runs anywhere)
- ✅ High performance
- ✅ Built-in Dependency Injection
- ✅ Modular (you only include what you need)
- ✅ Integrated middleware pipeline
- ✅ Unified for front-end + API + gRPC + minimal APIs

---

## 🧩 2. ASP.NET Core Architecture Overview

When a request hits your Web API:

[Client] → [Kestrel Server] → [Middleware Pipeline] → [Routing] → [Controller Action] → [Response]


Let’s break it down:

| Component                     | Description                                                       |
|-------------------------------|-------------------------------------------------------------------|
| **Kestrel**                   | The built-in web server that receives HTTP requests.              |
| **Middleware**                | Pieces of logic that inspect/modify requests and responses.       |
| **Routing**                   | Determines which controller and action should handle the request. |
| **Controllers**               | Contain your API logic (business endpoints).                      |
| **Dependency Injection (DI)** | Provides objects and services automatically.                      |
| **Response**                  | The data sent back to the client (JSON, XML, etc.).               |

---

## ⚙️ 3. ASP.NET Core Request Pipeline (Middleware Pipeline)

Every incoming request **passes through a pipeline** of middleware components.  
Each middleware can:
- Process the request
- Modify it
- Pass it to the next middleware
- Or short-circuit and send a response directly

Example:

Request → Authentication → Routing → Endpoint → Response


---

## 🧩 4. What is Middleware?

Middleware is **a software component** that handles requests and responses in the ASP.NET Core pipeline.

Each middleware component:
- Does something (logging, authentication, etc.)
- Calls the **next middleware** in the chain

### Example Concept:

```csharp
app.Use(async (context, next) =>
{
    Console.WriteLine("Before next middleware");
    await next.Invoke();
    Console.WriteLine("After next middleware");
});
```

Output when request runs:

```lua
Before next middleware
After next middleware
```
💡 The pipeline runs in sequence, top to bottom, in Program.cs.

## ⚙️ 5. Built-in Middleware Examples

| Middleware              | Purpose                                |
| ----------------------- | -------------------------------------- |
| `UseRouting()`          | Enables endpoint routing               |
| `UseAuthentication()`   | Validates user identity                |
| `UseAuthorization()`    | Checks access rights                   |
| `UseHttpsRedirection()` | Redirects HTTP → HTTPS                 |
| `UseCors()`             | Enables Cross-Origin requests          |
| `UseStaticFiles()`      | Serves static files (like images, CSS) |
| `UseExceptionHandler()` | Handles global exceptions              |

## 🧩 6. Order of Middleware Execution

Middleware order matters a lot.
Each is executed in the order it’s added.

Example:

```csharp
var app = builder.Build();

app.UseHttpsRedirection();
app.UseRouting();
app.UseAuthorization();

app.MapControllers();
app.Run();
```

If you change the order — say, put UseAuthorization() before UseRouting() — the app won’t work correctly because routing must happen before authorization.

## ⚙️ 7. Custom Middleware Example

You can create your own middleware to perform logging, timing, etc.

```csharp
public class RequestLoggingMiddleware
{
    private readonly RequestDelegate _next;
    public RequestLoggingMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        Console.WriteLine($"➡️ Request: {context.Request.Method} {context.Request.Path}");
        await _next(context); // Call next middleware
        Console.WriteLine($"⬅️ Response: {context.Response.StatusCode}");
    }
}
```

Register in Program.cs
```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.UseMiddleware<RequestLoggingMiddleware>();
app.MapGet("/", () => "Hello Middleware!");

app.Run();
```

Output:
```yaml
➡️ Request: GET /
⬅️ Response: 200
```

# 🧩 8. Difference: Middleware vs Controllers

| Aspect   | Middleware                       | Controller                          |
| -------- | -------------------------------- | ----------------------------------- |
| Purpose  | Cross-cutting concerns           | Business logic per resource         |
| Scope    | Global (applies to all requests) | Specific (based on routes)          |
| Examples | Logging, Auth, CORS              | ProductsController, UsersController |


## ⚙️ 9. How Middleware is Registered (Program.cs in .NET 8)

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();

var app = builder.Build();

app.UseHttpsRedirection();
app.UseAuthorization();

app.MapControllers();  // endpoint routing

app.Run();
```

Here:

1. UseHttpsRedirection() → built-in middleware
2. UseAuthorization() → middleware for auth
3. MapControllers() → final endpoint (controllers)

Everything runs in order.