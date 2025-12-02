# 🧠 Day 23 – Using DTOs (Data Transfer Objects) and ViewModels in ASP.NET Core

## 🎯 Goal for Today

By the end of this lesson, you’ll understand:

- What DTOs and ViewModels are, and why they’re used
- The difference between Entities, DTOs, and ViewModels
- How to create and map DTOs manually and with AutoMapper
- How DTOs improve security, performance, and code maintainability

## ⚙️ 1. Why We Need DTOs and ViewModels

When building APIs, we often deal with domain entities — classes directly mapped to the database.
However, sending these entities directly to the client is bad practice because:

- They may expose sensitive data (e.g., passwords, internal IDs).
- They can cause circular references in JSON serialization.
- Any change in entity structure might break the API contract.

✅ Solution: use Data Transfer Objects (DTOs) and ViewModels.

## 🧩 2. Entities vs DTOs vs ViewModels

| Type          | Purpose                                                            | Example                    |
| ------------- | ------------------------------------------------------------------ | -------------------------- |
| **Entity**    | Represents database table structure                                | `Product`, `User`, `Order` |
| **DTO**       | Simplifies and controls data transfer between client and server    | `ProductDTO`, `UserDTO`    |
| **ViewModel** | Used for *presentation layer* (especially in MVC or combined data) | `OrderDetailsViewModel`    |


Example: Entity

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public decimal Price { get; set; }
    public string SecretCode { get; set; } = string.Empty; // Should not go to client
    public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
}
```

DTO
```csharp
public class ProductDTO
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

Example: DTO (for input)

```csharp
public class CreateProductDTO
{
    public string Name { get; set; } = string.Empty;
    public decimal Price { get; set; }
}
```

## 🧱 3. Manual Mapping Example

Controller using DTOs:

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private readonly AppDbContext _context;

    public ProductsController(AppDbContext context)
    {
        _context = context;
    }

    [HttpGet]
    public async Task<ActionResult<IEnumerable<ProductDTO>>> GetAll()
    {
        var products = await _context.Products
            .Select(p => new ProductDTO
            {
                Id = p.Id,
                Name = p.Name,
                Price = p.Price
            })
            .ToListAsync();

        return Ok(products);
    }

    [HttpPost]
    public async Task<ActionResult<ProductDTO>> Create(CreateProductDTO dto)
    {
        var product = new Product
        {
            Name = dto.Name,
            Price = dto.Price
        };

        _context.Products.Add(product);
        await _context.SaveChangesAsync();

        var result = new ProductDTO
        {
            Id = product.Id,
            Name = product.Name,
            Price = product.Price
        };

        return CreatedAtAction(nameof(GetAll), new { id = product.Id }, result);
    }
}
```

## ⚡ 4. Introducing AutoMapper

Manual mapping becomes repetitive.
AutoMapper is a powerful library that automatically maps between objects with similar property names.

Install Package

```bash
dotnet add package AutoMapper.Extensions.Microsoft.DependencyInjection
```

Configure AutoMapper

Create a profile in a folder like Mappings/ProductProfile.cs

```csharp
using AutoMapper;

public class ProductProfile : Profile
{
    public ProductProfile()
    {
        CreateMap<Product, ProductDTO>();          // Entity → DTO
        CreateMap<CreateProductDTO, Product>();    // DTO → Entity
    }
}
```

Register AutoMapper in Program.cs

```csharp
builder.Services.AddAutoMapper(typeof(Program).Assembly);
```

Use AutoMapper in Controller

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private readonly AppDbContext _context;
    private readonly IMapper _mapper;

    public ProductsController(AppDbContext context, IMapper mapper)
    {
        _context = context;
        _mapper = mapper;
    }

    [HttpGet]
    public async Task<ActionResult<IEnumerable<ProductDTO>>> GetAll()
    {
        var products = await _context.Products.ToListAsync();
        var productDtos = _mapper.Map<List<ProductDTO>>(products);
        return Ok(productDtos);
    }

    [HttpPost]
    public async Task<ActionResult<ProductDTO>> Create(CreateProductDTO dto)
    {
        var product = _mapper.Map<Product>(dto);
        _context.Products.Add(product);
        await _context.SaveChangesAsync();

        var productDto = _mapper.Map<ProductDTO>(product);
        return CreatedAtAction(nameof(GetAll), new { id = product.Id }, productDto);
    }
}
```

## 🧩 5. Advanced Mapping Scenarios
✅ Custom Property Mapping

```csharp
CreateMap<Product, ProductDTO>()
    .ForMember(dest => dest.Name,
               opt => opt.MapFrom(src => src.Name.ToUpper()));
```

✅ Ignoring Properties

```csharp
CreateMap<Product, ProductDTO>()
    .ForMember(dest => dest.Price, opt => opt.Ignore());
```

## 🧠 6. ViewModels vs DTOs

While DTOs are mainly for data transfer,
ViewModels are for combining multiple sources or adding computed fields.

Example:

```csharp
public class OrderDetailsViewModel
{
    public string ProductName { get; set; } = string.Empty;
    public string CustomerName { get; set; } = string.Empty;
    public decimal TotalPrice { get; set; }
    public DateTime OrderedOn { get; set; }
}
```

You can fill this ViewModel using LINQ joins or projections.

## ⚙️ 7. Benefits of Using DTOs

✅ Improves security — hides sensitive fields
✅ Enhances performance — sends only needed data
✅ Increases maintainability — entities can change freely
✅ Simplifies validation and testing

## 🧠 8. Common Mistakes

| Mistake                         | Problem                 | Solution                             |
| ------------------------------- | ----------------------- | ------------------------------------ |
| Returning entities directly     | Exposes internal data   | Always use DTOs                      |
| DTOs mirroring entities exactly | Adds unnecessary layers | Only create meaningful DTOs          |
| Mapping logic in controllers    | Hard to maintain        | Move mapping to profiles or services |