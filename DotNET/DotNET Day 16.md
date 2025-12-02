# 🗓️ Day 16 – Routing, Controllers, and HTTP Methods

## 🎯 Goal for Today
Learn how **ASP.NET Core routes HTTP requests** to your controllers and how to use **HTTP verbs** (`GET`, `POST`, `PUT`, `DELETE`) to handle API operations.

---

## 🧠 1. What is Routing?

**Routing** determines **how incoming HTTP requests are mapped** to controller actions.

In simple terms:
> URL → Controller → Action → Response

Example:

GET /api/products is mapped to:
```csharp
ProductController → Get()
```

## ⚙️ 2. How Routing Works

Routing is enabled in the pipeline with:

```csharp
app.UseRouting();
app.MapControllers();
```

Two main routing styles:

1. Convention-based routing – Traditional MVC style (used less in Web API)
2. Attribute routing – Route is defined using attributes directly on controllers/actions
✅ Most common in Web APIs

## 🧩 3. Attribute Routing Example

```csharp
using Microsoft.AspNetCore.Mvc;

namespace MyApi.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class ProductsController : ControllerBase
    {
        [HttpGet]
        public IActionResult GetAll()
        {
            return Ok(new[] { "Pen", "Book", "Pencil" });
        }

        [HttpGet("{id}")]
        public IActionResult GetById(int id)
        {
            return Ok($"Product #{id}");
        }
    }
}
```

1. [Route("api/[controller]")] → [controller] token becomes Products.
2. [HttpGet] → Responds to GET /api/products
3. [HttpGet("{id}")] → Responds to GET /api/products/2

---

## 🌐 4. Route Templates

You can use placeholders and route constraints.

| Example                        | Description              |
| ------------------------------ | ------------------------ |
| `[HttpGet("{id:int}")]`        | Matches only integer IDs |
| `[HttpGet("{name}")]`          | Matches string parameter |
| `[HttpGet("{id:int:min(1)}")]` | Matches IDs ≥ 1          |
| `[Route("api/v1/products")]`   | Custom route prefix      |

Example:
```csharp
[HttpGet("category/{categoryName}")]
public IActionResult GetByCategory(string categoryName)
{
    return Ok($"Products in {categoryName}");
}
```

## ⚙️ 5. Common HTTP Method Attributes

| Attribute      | Purpose                  | Example                  |
| -------------- | ------------------------ | ------------------------ |
| `[HttpGet]`    | Retrieve data            | `GET /api/products`      |
| `[HttpPost]`   | Create new resource      | `POST /api/products`     |
| `[HttpPut]`    | Update existing resource | `PUT /api/products/1`    |
| `[HttpDelete]` | Delete a resource        | `DELETE /api/products/1` |
| `[HttpPatch]`  | Partially update         | `PATCH /api/products/1`  |

## ⚙️ 6. Understanding Action Results

| Return Type         | Description      | Example                      |
| ------------------- | ---------------- | ---------------------------- |
| `Ok()`              | 200 OK with data | `return Ok(data);`           |
| `CreatedAtAction()` | 201 Created      | When a new resource is added |
| `NotFound()`        | 404 Not Found    | When resource doesn’t exist  |
| `BadRequest()`      | 400 Bad Request  | Invalid input                |
| `NoContent()`       | 204 No Content   | Successful update/delete     |

## 🧩 7. Using [FromBody], [FromRoute], [FromQuery]

These attributes define where ASP.NET should read the data from:

| Attribute     | Source              | Example                                       |
| ------------- | ------------------- | --------------------------------------------- |
| `[FromBody]`  | Request body (JSON) | `POST /api/products` with `{ "name": "Pen" }` |
| `[FromRoute]` | URL segment         | `/api/products/2`                             |
| `[FromQuery]` | Query string        | `/api/products?name=Pen`                      |