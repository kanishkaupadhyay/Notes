# 🗓️ Day 6 – Encapsulation, Polymorphism & Interfaces in C#

## 🎯 Goal for Today
Learn advanced OOP concepts in C#: **encapsulation, polymorphism**, and **interfaces** to build maintainable and reusable code.

---

## ⚙️ 1. Encapsulation

Encapsulation is the concept of **hiding internal details** and exposing only what’s necessary.

### 🔹 Example with Private Fields
```csharp
class BankAccount
{
    private double balance;

    public void Deposit(double amount)
    {
        if (amount > 0)
            balance += amount;
    }

    public double GetBalance()
    {
        return balance;
    }
}
```

🔹 Usage

```csharp
BankAccount account = new BankAccount();
account.Deposit(1000);
Console.WriteLine(account.GetBalance()); // Output: 1000
```

## 🧩 2. Properties for Encapsulation

C# provides properties to simplify encapsulation.

```csharp
class BankAccount
{
    private double balance;

    public double Balance
    {
        get { return balance; }
        private set { balance = value; } // Private set
    }

    public void Deposit(double amount)
    {
        if (amount > 0)
            Balance += amount;
    }
}
```

```csharp
BankAccount acc = new BankAccount();
acc.Deposit(500);
Console.WriteLine(acc.Balance);
```

## ⚙️ 3. Polymorphism

Polymorphism allows objects to take multiple forms. There are two main types in C#:

### 🔹 3.1 Compile-time (Method Overloading)

```csharp
class Calculator
{
    public int Add(int a, int b) => a + b;
    public double Add(double a, double b) => a + b;
}
```
```csharp
Calculator calc = new Calculator();
Console.WriteLine(calc.Add(5, 10));      // 15
Console.WriteLine(calc.Add(5.5, 4.5));   // 10
```

### 🔹 3.2 Run-time (Method Overriding)

```csharp
public class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("The animal makes a sound.");
    }
}

public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("The dog barks: Woof!");
    }
}

public class Cat : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("The cat meows: Meow!");
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        Animal myDog = new Dog();
        Animal myCat = new Cat();
        Animal genericAnimal = new Animal();

        myDog.MakeSound(); // Output: The dog barks: Woof!
        myCat.MakeSound(); // Output: The cat meows: Meow!
        genericAnimal.MakeSound(); // Output: The animal makes a sound.
    }
}
```

## 🧩 4. Interfaces

An interface defines a contract that classes must implement.

```csharp
interface IShape
{
    double Area();
    double Perimeter();
}

class Rectangle : IShape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public double Area() => Width * Height;
    public double Perimeter() => 2 * (Width + Height);
}
```

```csharp
IShape rect = new Rectangle { Width = 5, Height = 10 };
Console.WriteLine(rect.Area());       // 50
Console.WriteLine(rect.Perimeter());  // 30
```

💡 Note: Interfaces provide flexibility and enforce standardization.

## ⚙️ 4. Abstract Classes

An abstract class is a class that cannot be instantiated directly and may contain abstract methods (without implementation) as well as concrete methods (with implementation).

### 🔹 Example: Abstract Class and Method

```csharp
abstract class Animal
{
    public string Name { get; set; }

    // Abstract method (no implementation)
    public abstract void Speak();

    // Concrete method
    public void Eat()
    {
        Console.WriteLine($"{Name} is eating.");
    }
}

class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Bark");
    }
}

class Cat : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Meow");
    }
}
```

### 🔹 Using Abstract Class

```csharp
Animal myDog = new Dog { Name = "Rex" };
myDog.Speak(); // Output: Bark
myDog.Eat();   // Output: Rex is eating

Animal myCat = new Cat { Name = "Misty" };
myCat.Speak(); // Output: Meow
myCat.Eat();   // Output: Misty is eating
```

💡 Tip: Use abstract classes when you have shared functionality and also want to enforce certain methods to be implemented in derived classes.