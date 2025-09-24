# Introduction

## Table of Contents

1. [What's C#?](#whats-c)
2. [.NET SDK Installation](#net-sdk-installation)
3. [Getting Started](#getting-started)
   - [Hello World](#hello-world)
   - [Variables](#variables)
     - [Explicitly Typed](#explicitly-typed)
     - [Implicitly Typed](#implicitly-typed)
   - [Constants](#constants)
4. [Best Practices for Beginners](#best-practices-for-beginners)
5. [Common Debugging Techniques](#common-debugging-techniques)
6. [Essential .NET CLI Commands](#essential-net-cli-commands)
7. [Additional Resources](#additional-resources)

## What's C#?

C# is a high-level, general-purpose programming language developed by Microsoft as part of the .NET framework. It's an object-oriented language, meaning it uses objects to structure code and data, and is used to build a variety of applications.

C# is known for its ease of learning, strong community support, and ability to produce highly performant code. 

## .NET SDK Installation

The easiest way to have the .NET SDK installed in your personal computer is to download [Visual Studio](https://visualstudio.microsoft.com/). You can also use other IDE, but you will need to install the [SDK](https://dotnet.microsoft.com/en-us/download/visual-studio-sdks) manually.

## Getting Started

### Hello World

A sample C# program is show here.

```csharp
// See https://aka.ms/new-console-template for more information
Console.WriteLine("Hello, World!");
```

Run the program as below:

```bash
$  dotnet run Program.cs
```

### Variables

A variable is a symbol used for storing different values.

#### Explicitly Typed:

```csharp
string firstName = "Someone";
char userOption = 'A';
int gameScore = 123;
float percentage = 12.10f;
double portion = 4.556;
decimal particlesPerMillion = 123.4567m;
bool processedCustomer = true;
```

#### Implicitly Typed:

```csharp
var message = "Hello world!";
```

### Constants

A kind of variable that is immutable.

```csharp
const int ConstNum = 5;
```

## Best Practices for Beginners
```csharp
// Naming conventions
public class PersonManager          // PascalCase for classes
{
    public string FirstName { get; set; }  // PascalCase for properties
    private int _age;                       // camelCase with underscore for private fields
    
    public void UpdatePersonInfo(string firstName, int age)  // PascalCase for methods
    {
        string fullName = firstName;        // camelCase for local variables
        const int MaxAge = 120;            // PascalCase for constants
        
        // Use meaningful variable names
        bool isValidAge = age > 0 && age <= MaxAge;
        if (isValidAge)
        {
            FirstName = firstName;
            _age = age;
        }
    }
}

// Error handling basics
public class SafeCalculator
{
    public double SafeDivide(double dividend, double divisor)
    {
        try
        {
            if (divisor == 0)
                throw new ArgumentException("Cannot divide by zero");
                
            return dividend / divisor;
        }
        catch (ArgumentException ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
            return 0;
        }
        finally
        {
            Console.WriteLine("Division operation completed");
        }
    }
}

// Using nullable types safely
public class PersonInfo
{
    public string? MiddleName { get; set; }  // Nullable reference type
    public int? Age { get; set; }            // Nullable value type
    
    public string GetDisplayName(string firstName, string lastName)
    {
        // Safe handling of nullable values
        return MiddleName switch
        {
            null or "" => $"{firstName} {lastName}",
            _ => $"{firstName} {MiddleName} {lastName}"
        };
    }
    
    public bool IsAdult()
    {
        return Age >= 18;  // Null-conditional operator would be Age >= 18
    }
}
```

## Common Debugging Techniques
```csharp
// Debug output and logging
using System.Diagnostics;

public class DebuggingExample
{
    public void DemonstrateDebugging()
    {
        // Console output for simple debugging
        Console.WriteLine("Starting calculation...");
        
        // Debug output (only in debug builds)
        Debug.WriteLine("Debug: Entering calculation method");
        
        // Conditional compilation
        #if DEBUG
        Console.WriteLine("Running in debug mode");
        #endif
        
        // Using debugger breakpoints
        int x = 10;
        int y = 5;
        
        // Set breakpoint here and inspect variables
        int result = x + y;
        
        Console.WriteLine($"Result: {result}");
        
        // Assert for debugging (only in debug builds)
        Debug.Assert(result == 15, "Addition should equal 15");
    }
    
    public void HandleErrors()
    {
        try
        {
            string input = Console.ReadLine() ?? string.Empty;
            int number = int.Parse(input);
            Console.WriteLine($"You entered: {number}");
        }
        catch (FormatException)
        {
            Console.WriteLine("Please enter a valid number");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Unexpected error: {ex.Message}");
            Console.WriteLine($"Stack trace: {ex.StackTrace}");
        }
    }
}
```

## Essential .NET CLI Commands
```bash
# Create new projects
dotnet new console -n MyConsoleApp
dotnet new classlib -n MyLibrary
dotnet new webapi -n MyWebApi
dotnet new mvc -n MyMvcApp

# Build and run
dotnet build                    # Build the project
dotnet run                     # Run the project
dotnet run --project MyApp     # Run specific project

# Package management
dotnet add package Newtonsoft.Json
dotnet remove package OldPackage
dotnet restore                 # Restore packages

# Testing
dotnet new xunit -n MyTests    # Create test project
dotnet test                    # Run tests

# Publishing
dotnet publish -c Release      # Publish for production
dotnet publish --self-contained # Include runtime

# Information
dotnet --version              # Check .NET version
dotnet --info                # Detailed system info
dotnet list reference         # Show project references
```

## Additional Resources

### Official Documentation and Getting Started
- [Microsoft C# Documentation](https://docs.microsoft.com/en-us/dotnet/csharp/) - Official comprehensive C# documentation
- [C# Programming Guide](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/) - Step-by-step programming guide
- [.NET Download](https://dotnet.microsoft.com/download) - Download the latest .NET SDK
- [C# Language Reference](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/) - Complete language specification
- [What's New in C#](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/) - Latest C# features and updates

### Development Environment Setup
- [Visual Studio](https://visualstudio.microsoft.com/) - Full-featured IDE for Windows
- [Visual Studio Code](https://code.visualstudio.com/) - Lightweight, cross-platform editor
- [JetBrains Rider](https://www.jetbrains.com/rider/) - Professional cross-platform .NET IDE
- [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) - IDE for macOS developers
- [.NET CLI Commands](https://docs.microsoft.com/en-us/dotnet/core/tools/) - Command-line interface reference

### Learning Roadmap for Beginners
1. **Week 1-2: Basics**
   - Variables, data types, operators
   - Control flow (if/else, loops)
   - Methods and functions
   - Basic debugging

2. **Week 3-4: Object-Oriented Programming**
   - Classes and objects
   - Properties and methods
   - Inheritance and polymorphism
   - Interfaces and abstract classes

3. **Week 5-6: Collections and LINQ**
   - Arrays, Lists, Dictionaries
   - LINQ queries and methods
   - Working with data

4. **Week 7-8: Advanced Concepts**
   - Exception handling
   - File I/O operations
   - Async/await programming
   - Generics

5. **Week 9-12: Real-World Applications**
   - Database connectivity
   - Web APIs or desktop applications
   - Unit testing
   - Deployment and publishing

### Learning Resources
- **Interactive Learning:**
  - [Microsoft Learn C#](https://docs.microsoft.com/en-us/learn/paths/csharp-first-steps/) - Free interactive tutorials
  - [Codecademy C#](https://www.codecademy.com/learn/learn-c-sharp) - Hands-on coding exercises
  - [freeCodeCamp C#](https://www.freecodecamp.org/learn/) - Free certification course

- **Books:**
  - "C# 12 in a Nutshell" by Joseph Albahari - Comprehensive reference
  - "Head First C#" by Jennifer Greene - Beginner-friendly approach
  - "C# Player's Guide" by RB Whitaker - Game-based learning
  - "Effective C#" by Bill Wagner - Best practices and advanced concepts

- **Online Courses:**
  - [Pluralsight C# Path](https://www.pluralsight.com/paths/csharp) - Professional training
  - [Udemy C# Masterclass](https://www.udemy.com/topic/c-sharp/) - Practical projects

- **YouTube Channels:**
  - [IAmTimCorey](https://www.youtube.com/user/IAmTimCorey) - Practical C# tutorials
  - [Programming with Mosh](https://www.youtube.com/user/programmingwithmosh) - Beginner-friendly content
  - [Derek Banas](https://www.youtube.com/user/derekbanas) - Quick language overviews

### Practice Platforms
- [LeetCode](https://leetcode.com/) - Algorithm and data structure problems
- [HackerRank](https://www.hackerrank.com/domains/algorithms) - Programming challenges
- [Codewars](https://www.codewars.com/kata/search/csharp) - Code kata for skill building
- [Exercism](https://exercism.org/tracks/csharp) - Mentored practice problems
- [Project Euler](https://projecteuler.net/) - Mathematical programming challenges

### Development Tools and Extensions
- **Visual Studio Extensions:**
  - ReSharper - Code analysis and refactoring
  - CodeMaid - Code cleanup and organization
  - Visual Studio IntelliCode - AI-assisted development

- **VS Code Extensions:**
  - C# Dev Kit - Official C# extension pack
  - C# Extensions - Additional C# utilities
  - .NET Install Tool - Manage .NET versions
  - REST Client - Test APIs

- **Useful NuGet Packages for Beginners:**
  - [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) - JSON serialization
  - [Serilog](https://www.nuget.org/packages/Serilog/) - Structured logging
  - [FluentAssertions](https://www.nuget.org/packages/FluentAssertions/) - Better test assertions
  - [Bogus](https://www.nuget.org/packages/Bogus/) - Fake data generation

### Community and Support
- [Stack Overflow C#](https://stackoverflow.com/questions/tagged/c%23) - Q&A community
- [Reddit r/csharp](https://www.reddit.com/r/csharp/) - Community discussions
- [C# Discord Server](https://discord.gg/csharp) - Real-time help and chat
- [.NET Foundation](https://dotnetfoundation.org/) - Official .NET community
- [Microsoft Developer Community](https://developercommunity.visualstudio.com/) - Report issues and suggestions
- [GitHub Discussions](https://github.com/dotnet/core/discussions) - .NET community discussions

### Next Steps After Introduction
1. **Choose a Project Type:**
   - Console applications for learning basics
   - Desktop apps (WPF, WinUI, MAUI)
   - Web applications (ASP.NET Core)
   - APIs and microservices
   - Mobile apps (MAUI, Xamarin)

2. **Set Up Version Control:**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   ```

3. **Learn Testing:**
   ```bash
   dotnet new xunit -n MyProject.Tests
   dotnet add reference ../MyProject/MyProject.csproj
   ```

4. **Explore Frameworks:**
   - ASP.NET Core for web development
   - Entity Framework Core for databases
   - Xamarin/MAUI for mobile apps
   - Unity for game development

Remember: The best way to learn C# is by building projects! Start small, practice regularly, and don't hesitate to ask for help in the community.