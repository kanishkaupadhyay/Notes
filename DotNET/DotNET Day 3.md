# 🗓️ Day 3 – Operators, Control Flow & Loops in C#

## 🎯 Goal for Today
Understand how C# handles operations, decisions, and repetitions — the logic backbone of every program.

---

## ⚙️ 1. Operators in C#

Operators are symbols that perform operations on variables and values.

### 🔹 Arithmetic Operators
| Operator | Description | Example |
|-----------|--------------|----------|
| `+` | Addition | `a + b` |
| `-` | Subtraction | `a - b` |
| `*` | Multiplication | `a * b` |
| `/` | Division | `a / b` |
| `%` | Modulus (remainder) | `a % b` |

```csharp
int a = 10, b = 3;
Console.WriteLine(a + b); // 13
Console.WriteLine(a % b); // 1
```

---

### 🔸 Assignment Operators
| Operator | Example | Equivalent To |
|-----------|----------|----------------|
| `=` | `x = 10` | Assign value |
| `+=` | `x += 5` | `x = x + 5` |
| `-=` | `x -= 2` | `x = x - 2` |
| `*=` | `x *= 3` | `x = x * 3` |
| `/=` | `x /= 2` | `x = x / 2` |

---

### 🔹 Comparison Operators
Used in conditions.

| Operator | Description | Example |
|-----------|-------------|----------|
| `==` | Equal to | `a == b` |
| `!=` | Not equal to | `a != b` |
| `>` | Greater than | `a > b` |
| `<` | Less than | `a < b` |
| `>=` | Greater or equal | `a >= b` |
| `<=` | Less or equal | `a <= b` |

---

### 🔸 Logical Operators
Used to combine conditions.

| Operator | Description | Example |
|-----------|-------------|----------|
| `&&` | AND | `a > 10 && b < 5` |
| `||` | OR | `a > 10 || b < 5` |
| `!` | NOT | `!(a > 10)` |

---

### 🔹 Unary Operators
| Operator | Description | Example |
|-----------|-------------|----------|
| `++` | Increment | `x++` or `++x` |
| `--` | Decrement | `x--` or `--x` |

---

### 🔸 Ternary Operator
```csharp
int age = 18;
string result = (age >= 18) ? "Adult" : "Minor";
Console.WriteLine(result);
```

---

## 🔍 2. Control Flow – Decision Making

### 🧩 `if` and `else` Statements
```csharp
int age = 20;

if (age >= 18)
{
    Console.WriteLine("Eligible to vote");
}
else
{
    Console.WriteLine("Not eligible to vote");
}
```

---

### 🧩 `else if` Ladder
```csharp
int marks = 75;

if (marks >= 90)
    Console.WriteLine("Excellent");
else if (marks >= 75)
    Console.WriteLine("Good");
else if (marks >= 50)
    Console.WriteLine("Average");
else
    Console.WriteLine("Fail");
```

---

### 🧩 `switch` Statement
```csharp
int day = 3;

switch (day)
{
    case 1:
        Console.WriteLine("Monday");
        break;
    case 2:
        Console.WriteLine("Tuesday");
        break;
    case 3:
        Console.WriteLine("Wednesday");
        break;
    default:
        Console.WriteLine("Invalid day");
        break;
}
```

💡 **Note:** You can also use **switch expressions** in modern C#:
```csharp
string result = day switch
{
    1 => "Monday",
    2 => "Tuesday",
    3 => "Wednesday",
    _ => "Invalid"
};
Console.WriteLine(result);
```

---

## 🔁 3. Loops in C#

### 🔹 `for` Loop
Used when you know how many times to iterate.
```csharp
for (int i = 1; i <= 5; i++)
{
    Console.WriteLine($"Iteration {i}");
}
```

---

### 🔸 `while` Loop
Used when you don’t know how many times to repeat.
```csharp
int i = 1;
while (i <= 5)
{
    Console.WriteLine(i);
    i++;
}
```

---

### 🔹 `do-while` Loop
Executes **at least once** before checking the condition.
```csharp
int j = 1;
do
{
    Console.WriteLine(j);
    j++;
} while (j <= 5);
```

---

### 🔸 `foreach` Loop
Used for collections or arrays.
```csharp
string[] fruits = { "Apple", "Banana", "Mango" };

foreach (var fruit in fruits)
{
    Console.WriteLine(fruit);
}
```

---

### 🔹 `break` and `continue`
```csharp
for (int i = 1; i <= 10; i++)
{
    if (i == 5)
        continue; // skips iteration
    if (i == 8)
        break; // exits loop
    Console.WriteLine(i);
}
```

---

## 🧩 4. Nested Loops Example
```csharp
for (int i = 1; i <= 3; i++)
{
    for (int j = 1; j <= 2; j++)
    {
        Console.WriteLine($"i={i}, j={j}");
    }
}
```

---

## ✅ Summary

By the end of Day 3, you should:
- Understand and use arithmetic, comparison, and logical operators  
- Use `if`, `else if`, and `switch` for decisions  
- Use loops (`for`, `while`, `do-while`, `foreach`) effectively  
- Control loop execution with `break` and `continue`  

---