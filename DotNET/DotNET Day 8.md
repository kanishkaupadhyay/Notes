# 🗓️ Day 8 – Collections and Generics in C#

## 🎯 Goal for Today
Understand the **Collections Framework** and **Generics** in C#, how to use them efficiently, and how they enhance performance, type safety, and reusability.

---

## ⚙️ 1. Understanding Collections

Collections are **containers that hold multiple items (objects)**.  
They are more flexible than arrays and part of the `System.Collections` and `System.Collections.Generic` namespaces.

### 🔹 1.1 Non-Generic Collections
Before generics, we had older collection types that store elements as `object`, e.g.:
- `ArrayList`
- `Hashtable`
- `Queue`
- `Stack`

However, they are **not type-safe** and require **boxing/unboxing**.

```csharp
using System.Collections;

ArrayList list = new ArrayList();
list.Add(10);
list.Add("Hello"); // Allowed but not safe

foreach (var item in list)
{
    Console.WriteLine(item);
}
```

## ⚙️ 2. Generic Collections (Type-Safe)

Introduced in .NET 2.0, Generics let you define classes and methods that work with any data type safely and efficiently.

Common generic collections from System.Collections.Generic:

| Type                       | Description                      |
| -------------------------- | -------------------------------- |
| `List<T>`                  | Dynamic array, supports indexing |
| `Dictionary<TKey, TValue>` | Key-value pair collection        |
| `Queue<T>`                 | FIFO (First In, First Out)       |
| `Stack<T>`                 | LIFO (Last In, First Out)        |
| `HashSet<T>`               | Unique unordered items           |

## ⚙️ 3. Understanding Generics

Generics allow creating type-safe, reusable classes, interfaces, and methods.

### 🔹 3.1 Generic Class Example

```csharp
class Storage<T>
{
    private T item;

    public void Store(T value)
    {
        item = value;
    }

    public T Retrieve()
    {
        return item;
    }
}
```

```csharp
Storage<int> intStorage = new Storage<int>();
intStorage.Store(100);
Console.WriteLine(intStorage.Retrieve());

Storage<string> stringStorage = new Storage<string>();
stringStorage.Store("Hello Generics!");
Console.WriteLine(stringStorage.Retrieve());
```

💡 Benefits:

1. Type Safety (compile-time)

2. Reusability

3. Performance (no boxing/unboxing)

### 🔹 3.2 Generic Methods

```csharp
class Utilities
{
    public static void Display<T>(T data)
    {
        Console.WriteLine($"Data: {data}");
    }
}

Utilities.Display<int>(42);
Utilities.Display("Generic Method Example");
```

### 🔹 3.3 Generic Constraints

You can restrict generic types using constraints:

| Constraint            | Meaning                               |
| --------------------- | ------------------------------------- |
| `where T : class`     | Must be a reference type              |
| `where T : struct`    | Must be a value type                  |
| `where T : new()`     | Must have a parameterless constructor |
| `where T : BaseClass` | Must inherit from a specific class    |
| `where T : Interface` | Must implement an interface           |

```csharp
class Factory<T> where T : new()
{
    public T CreateInstance()
    {
        return new T();
    }
}

Factory<StringBuilder> factory = new Factory<StringBuilder>();
var sb = factory.CreateInstance();
```

### ⚙️ 4. Comparison – Array vs List vs Dictionary

| Feature           | Array       | List<T> | Dictionary<TKey, TValue> |
| ----------------- | ----------- | ------- | ------------------------ |
| Size              | Fixed       | Dynamic | Dynamic                  |
| Access by index   | ✅           | ✅       | ❌ (by key)               |
| Key-value storage | ❌           | ❌       | ✅                        |
| Type-safe         | ✅           | ✅       | ✅                        |
| LINQ Support      | ✅ (partial) | ✅       | ✅                        |
