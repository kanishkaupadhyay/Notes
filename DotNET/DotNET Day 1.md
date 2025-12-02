# 🗓️ Day 1 – Overview of .NET, .NET Core, and .NET 8

## 🎯 Goal for Today
Understand what .NET actually is, how it evolved into .NET Core and now unified as .NET 8, and what tools and components make up the ecosystem.

---

## 🧩 1. What is .NET?

**.NET** is a **free, open-source, cross-platform** framework created by Microsoft for building:

- Web applications and APIs (**ASP.NET Core**)
- Desktop apps (WPF, WinForms, MAUI)
- Mobile apps (MAUI, Xamarin)
- Cloud services, IoT, and AI integrations

### 🏗️ Components of .NET

| Component | Description |
|------------|--------------|
| **CLR (Common Language Runtime)** | Executes your code and manages memory, threads, and garbage collection |
| **BCL (Base Class Library)** | Ready-to-use classes for collections, file I/O, data types, etc. |
| **C# / F# / VB.NET** | Supported programming languages |
| **SDK & Runtime** | The tools and runtime environment for compiling and running .NET apps |

---

## 🧬 2. Evolution of .NET

| Generation | Year | Key Highlights |
|-------------|------|----------------|
| **.NET Framework** | 2002 | Windows-only; monolithic |
| **.NET Core (1.0–3.1)** | 2016 | Cross-platform, open source |
| **.NET 5 / 6 / 7** | 2020–2023 | Unified platform; improved performance |
| **.NET 8 (Current LTS)** | 2024 | Better performance, native AOT, modern APIs |

✅ **.NET 8** is the **LTS (Long-Term Support)** version — stable for production.

---

## ⚙️ 3. What is ASP.NET Core?

It’s the **web application framework** built on .NET Core.  
Use it to build:

- RESTful Web APIs  
- Web Apps (MVC or Razor Pages)  
- Real-time apps (SignalR)

### ✨ Features

- Cross-platform  
- High performance (Kestrel server)  
- Built-in Dependency Injection  
- Middleware-based request pipeline  
- Unified for Minimal APIs, MVC, and Razor

---

## 🧰 4. Developer Tools You’ll Use

- **Visual Studio 2022** or **Visual Studio Code**
- **.NET SDK 8.0+**  
  Verify installation:
  ```bash
  dotnet --version