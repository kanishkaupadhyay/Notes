# 🗓️ Day 4 – Arrays, Lists & Collections in C#

## 🎯 Goal for Today
Learn how to store, access, and manage groups of data using arrays, lists, and collections.

---

## ⚙️ 1. Arrays in C#

An **array** is a collection of elements of the same type stored in contiguous memory.

### 🔹 Declaring and Initializing Arrays
```csharp
int[] numbers = new int[5]; // Declare an array of size 5
numbers[0] = 10;
numbers[1] = 20;

int[] values = { 1, 2, 3, 4, 5 }; // Inline initialization
```

### 🔹 Accessing Array Elements

```csharp
Console.WriteLine(values[0]); // Output: 1
Console.WriteLine(values.Length); // Output: 5
```

### 🔹 Looping through Arrays

```csharp
for (int i = 0; i < values.Length; i++)
{
    Console.WriteLine(values[i]);
}

foreach (int num in values)
{
    Console.WriteLine(num);
}
```
### 🔸 Multi-Dimensional Arrays

```csharp
int[,] matrix = new int[2, 3]
{
    { 1, 2, 3 },
    { 4, 5, 6 }
};

Console.WriteLine(matrix[1, 2]); // Output: 6
```

### 🔸 Jagged Arrays (Arrays of Arrays)

```csharp
int[][] jaggedArray = new int[2][];
jaggedArray[0] = new int[] { 1, 2 };
jaggedArray[1] = new int[] { 3, 4, 5 };

Console.WriteLine(jaggedArray[1][2]); // Output: 5
```

## 🧩 2. Lists in C#

List<T> is a dynamic collection — it can grow or shrink in size.

### 🔹 Declaration, Adding & Removing Items, Accessing Elements and Iterating a List

```csharp
using System.Collections.Generic;

List<string> fruits = new List<string> { "Apple", "Banana", "Mango" };

fruits.Add("Orange");
fruits.Remove("Banana");

Console.WriteLine(fruits[0]); // Output: Apple
Console.WriteLine(fruits.Count); // Output: 3

foreach (var fruit in fruits)
{
    Console.WriteLine(fruit);
}
```

### 🔸 Useful Methods

| Method                | Description              |
| --------------------- | ------------------------ |
| `Add(item)`           | Adds item to end         |
| `Insert(index, item)` | Inserts item at position |
| `Remove(item)`        | Removes specific item    |
| `RemoveAt(index)`     | Removes by index         |
| `Contains(item)`      | Checks if item exists    |
| `Sort()`              | Sorts the list           |
| `Clear()`             | Removes all items        |


## 🧠 3. Collections in C#

Collections are more flexible than arrays and come from System.Collections or System.Collections.Generic.

### 🔹 Dictionary<TKey, TValue>

Stores data as key-value pairs.

```csharp
var employee = new Dictionary<int, string>
{
    { 101, "John" },
    { 102, "Mary" },
    { 103, "Steve" }
};

Console.WriteLine(employee[102]); // Output: Mary

foreach (var kvp in employee)
{
    Console.WriteLine($"ID: {kvp.Key}, Name: {kvp.Value}");
}
```

### 🔸 HashSet<T>

Stores unique elements (no duplicates).

```csharp
var numbers = new HashSet<int> { 1, 2, 3, 3, 2 };
foreach (var num in numbers)
{
    Console.WriteLine(num); // Output: 1 2 3
}
```

### 🔹 Queue<T>

```csharp
FIFO (First-In-First-Out) collection.

Queue<string> queue = new Queue<string>();
queue.Enqueue("A");
queue.Enqueue("B");
queue.Enqueue("C");

Console.WriteLine(queue.Dequeue()); // Output: A
```

### 🔸 Stack<T>

LIFO (Last-In-First-Out) collection.

```csharp
Stack<int> stack = new Stack<int>();
stack.Push(1);
stack.Push(2);
stack.Push(3);

Console.WriteLine(stack.Pop()); // Output: 3
```

### 🧩 4. LINQ with Collections (Preview)

You can perform powerful queries using LINQ (Language Integrated Query).

```csharp
var evenNumbers = values.Where(x => x % 2 == 0).ToList();

foreach (var n in evenNumbers)
{
    Console.WriteLine(n);
}
```