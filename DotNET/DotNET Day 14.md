# 🗓️ Day 15 – Introduction to ASP.NET Core Web API

## 🎯 Goal for Today
Get introduced to **ASP.NET Core Web API**, understand how it works, and set up your first **Web API project** in .NET 8.

---

## 📘 1. What is a Web API?

A **Web API** (Application Programming Interface) allows applications to **communicate over HTTP** — just like websites, but for data instead of pages.

In .NET, **ASP.NET Core Web API** is a framework that lets you:
- Build RESTful APIs (GET, POST, PUT, DELETE)
- Handle JSON requests/responses
- Support authentication, versioning, dependency injection, etc.

---

## 🌐 2. REST Principles

REST (Representational State Transfer) is a set of conventions for building web APIs.

| HTTP Verb  | Description                 | Example |
|------------|-----------------------------|----------|
| GET        | Retrieve data               | `/api/products` |
| POST       | Create a new resource       | `/api/products` |
| PUT        | Update an existing resource | `/api/products/1` |
| DELETE     | Remove a resource           | `/api/products/1` |

REST APIs are **stateless**, **resource-oriented**, and typically return data in **JSON**.

---

## ⚙️ 3. Tools You’ll Need

✅ Install the following:
- [.NET 8 SDK](https://dotnet.microsoft.com/download)
- Visual Studio 2022 / VS Code
- Postman (for testing APIs)
- Optional: SQL Server (for later database integration)

---

## 🧩 4. Creating Your First Web API Project

Run this command in your terminal:

```bash
dotnet new webapi -n FirstWebAPI
cd FirstWebAPI
```

This creates a complete starter API.

## 🗂️ 5. Project Structure Overview

FirstWebAPI/
│
├── Controllers/
│   └── WeatherForecastController.cs
│
├── Program.cs
├── appsettings.json
└── Properties/

| File/Folder                      | Description                                                      |
| -------------------------------- | ---------------------------------------------------------------- |
| `Controllers/`                   | Contains all API controllers (each handles a specific resource). |
| `Program.cs`                     | Entry point; configures services, middleware, and endpoints.     |
| `appsettings.json`               | Configuration file (e.g., DB connection strings).                |
| `Properties/launchSettings.json` | Contains URLs and profile settings for running the app.          |

## 🧠 6. Understanding Program.cs

Here’s the basic setup of a .NET 8 Web API:

```csharp
var builder = WebApplication.CreateBuilder(args);

// Register services
builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Enable Swagger in development
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();
app.UseAuthorization();

// Map controller endpoints
app.MapControllers();

app.Run();
```

Key Components:

1. AddControllers() → Enables MVC controller support.

2. SwaggerGen() → Generates interactive API documentation.

3. MapControllers() → Connects controller endpoints to the HTTP pipeline.

4. app.Run() → Starts the application.

## 🌦️ 7. The Default WeatherForecast Controller

When you open your project, you’ll see a sample controller:

```csharp
using Microsoft.AspNetCore.Mvc;

namespace FirstWebAPI.Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class WeatherForecastController : ControllerBase
    {
        private static readonly string[] Summaries = new[]
        {
            "Freezing", "Bracing", "Chilly", "Cool", "Mild",
            "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
        };

        [HttpGet]
        public IEnumerable<WeatherForecast> Get()
        {
            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = DateTime.Now.AddDays(index),
                TemperatureC = Random.Shared.Next(-20, 55),
                Summary = Summaries[Random.Shared.Next(Summaries.Length)]
            });
        }
    }

    public class WeatherForecast
    {
        public DateTime Date { get; set; }
        public int TemperatureC { get; set; }
        public string? Summary { get; set; }
    }
}
```

Notes:
1. [ApiController] attribute enforces automatic model validation and JSON handling.
2. [Route("[controller]")] → The route becomes /WeatherForecast.
3. [HttpGet] defines the HTTP verb this method responds to.

Run the project and visit:

```bash
https://localhost:5001/swagger
```

You’ll see the Swagger UI where you can test your GET endpoint.

## 🧠 8. Key Concepts Recap

| Concept                         | Description                                          |
| ------------------------------- | ---------------------------------------------------- |
| `ControllerBase`                | Base class for API controllers (no views).           |
| `[Route]`                       | Defines the URL pattern for your controller actions. |
| `[HttpGet]`, `[HttpPost]`, etc. | Define which HTTP method is supported.               |
| `ActionResult<T>`               | Returns data with HTTP status codes.                 |
| `Swagger`                       | Built-in API testing/documentation tool.             |
