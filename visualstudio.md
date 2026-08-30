# Visual Studio & .NET Development Guide

A comprehensive productivity guide for Visual Studio IDE, C#, and .NET Core / .NET 8+ development.

---

## 1. Quick Links & Resources

- **Visual Studio Benefits & Subscriptions:** [https://my.visualstudio.com/benefits](https://my.visualstudio.com/benefits)
- **.NET Downloads & SDKs:** [https://dotnet.microsoft.com/download](https://dotnet.microsoft.com/download)
- **Microsoft Learn .NET Docs:** [https://learn.microsoft.com/en-us/dotnet/](https://learn.microsoft.com/en-us/dotnet/)
- **C# Language Reference:** [https://learn.microsoft.com/en-us/dotnet/csharp/](https://learn.microsoft.com/en-us/dotnet/csharp/)

---

## 2. Essential Visual Studio Shortcuts (Windows)

### Navigation & Code Browsing
| Action | Shortcut |
| :--- | :--- |
| **Go to All / Quick Search** | `Ctrl + T` or `Ctrl + ,` |
| **Go to Definition** | `F12` |
| **Peek Definition** | `Alt + F12` |
| **Go to Implementation** | `Ctrl + F12` |
| **Find All References** | `Shift + F12` |
| **Sync with Active Document** | `Shift + Alt + L` |
| **Navigate Backward / Forward** | `Ctrl + -` / `Ctrl + Shift + -` |
| **Quick Actions / Refactoring** | `Ctrl + .` or `Alt + Enter` |

### Editing & Code Generation
| Action | Shortcut |
| :--- | :--- |
| **Format Document** | `Ctrl + K, Ctrl + D` |
| **Format Selection** | `Ctrl + K, Ctrl + F` |
| **Comment Selection** | `Ctrl + K, Ctrl + C` |
| **Uncomment Selection** | `Ctrl + K, Ctrl + U` |
| **Duplicate Line** | `Ctrl + D` |
| **Delete Line** | `Ctrl + Shift + L` |
| **Move Line Up / Down** | `Alt + Up` / `Alt + Down` |
| **Box / Multi-Line Selection** | `Shift + Alt + Arrow Keys` |
| **Surround With...** | `Ctrl + K, Ctrl + S` (e.g. `try-catch`, `#region`) |

### Popular Code Snippets (Type word + press `Tab` twice)
- `prop` $\rightarrow$ Auto-implemented property `public int MyProperty { get; set; }`
- `propg` $\rightarrow$ Property with private setter `public int MyProperty { get; private set; }`
- `ctor` $\rightarrow$ Constructor for the containing class
- `cw` $\rightarrow$ `Console.WriteLine();`
- `try` $\rightarrow$ `try ... catch` block
- `tryf` $\rightarrow$ `try ... finally` block
- `foreach` $\rightarrow$ `foreach (...)` loop
- `sim` $\rightarrow$ `static int Main(string[] args)`

---

## 3. Debugging & Diagnostics

### Debugging Keys
| Action | Shortcut |
| :--- | :--- |
| **Start Debugging** | `F5` |
| **Start Without Debugging** | `Ctrl + F5` |
| **Stop Debugging** | `Shift + F5` |
| **Restart Debugging** | `Ctrl + Shift + F5` |
| **Step Over** | `F10` |
| **Step Into** | `F11` |
| **Step Out** | `Shift + F11` |
| **Toggle Breakpoint** | `F9` |
| **Hot Reload** | `Alt + F10` |
| **Attach to Process** | `Ctrl + Alt + P` |

### Advanced Debugging Features
- **Conditional Breakpoint:** Right-click breakpoint red dot $\rightarrow$ **Conditions** $\rightarrow$ e.g., `userId == 123` or hit count > 10.
- **Tracepoints (No-Code Logging):** Right-click breakpoint $\rightarrow$ **Actions** $\rightarrow$ Log a message to Output window without stopping execution (e.g., `"Value: {item.Name}"`).
- **Immediate Window (`Ctrl + Alt + I`):** Evaluate expressions, inspect variables, or call methods directly while paused at a breakpoint.
- **Run to Cursor (`Ctrl + F10`):** Run the debugger directly up to the line where your caret is placed.
- **Set Next Statement (`Ctrl + Shift + F10`):** Move the instruction pointer backwards or forwards without re-running code.

---

## 4. Essential .NET CLI Commands (`dotnet`)

Run these commands inside Visual Studio Developer PowerShell or Windows Terminal:

### Project & Solution Management
```bash
# Create a new solution and projects
dotnet new sln -n MyApp
dotnet new webapi -n MyApp.Api
dotnet new classlib -n MyApp.Core
dotnet new xunit -n MyApp.Tests

# Add projects to solution
dotnet sln add MyApp.Api/MyApp.Api.csproj
dotnet sln add MyApp.Core/MyApp.Core.csproj

# Add project-to-project reference
dotnet add MyApp.Api/MyApp.Api.csproj reference MyApp.Core/MyApp.Core.csproj
```

### Building, Running & Hot Reload
```bash
# Build solution
dotnet build

# Run project with hot reload (auto restarts on file changes)
dotnet watch --project MyApp.Api

# Run tests with detailed console log
dotnet test --logger "console;verbosity=detailed"
```

### NuGet Package Management
```bash
# Add a NuGet package
dotnet add package Newtonsoft.Json
dotnet add package Microsoft.EntityFrameworkCore.SqlServer

# Check for outdated dependencies
dotnet list package --outdated

# Restore packages
dotnet restore
```

### Entity Framework Core CLI (`dotnet-ef`)
```bash
# Install EF Core tool globally
dotnet tool install --global dotnet-ef

# Add a migration
dotnet ef migrations add InitialCreate --project MyApp.Data --startup-project MyApp.Api

# Apply migrations to database
dotnet ef database update --project MyApp.Data --startup-project MyApp.Api

# Scaffold DbContext from an existing database (Database-First)
dotnet ef dbcontext scaffold "Server=localhost;Database=MyDb;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

---

## 5. .NET Coding Best Practices & Performance Tips

1. **Async / Await:**
   - Always accept and forward `CancellationToken` in async controller methods and database calls to cancel operations when a client disconnects.
   - Avoid `Task.Result` or `.Wait()` because they can lead to thread pool starvation and deadlocks.

2. **HttpClient Best Practice:**
   - Never instantiate `new HttpClient()` inside short-lived methods (leads to socket exhaustion).
   - Register it via `builder.Services.AddHttpClient()` and inject `IHttpClientFactory` or use typed clients.

3. **Memory & Allocations:**
   - Use `StringBuilder` for heavy string concatenations in loops.
   - Use `Span<T>` and `ReadOnlySpan<T>` for slicing strings or byte buffers without allocating memory on the heap.

4. **Task List Comments:**
   - Use tags in comments to automatically populate the Visual Studio **Task List** (`Ctrl + \ , T`):
     - `// TODO: implement caching here`
     - `// HACK: temporary fix for legacy payload`
     - `// UNDONE: finish validation logic`

5. **Quick Clean-Up Script:**
   Delete all `bin` and `obj` folders across a solution in PowerShell:
   ```powershell
   Get-ChildItem -Include bin,obj -Recurse | Remove-Item -Force -Recurse
   ```