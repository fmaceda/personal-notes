# Flow Control

## Table of Content

1. [Conditions](#conditions)
   - [if-else operator](#if-else-operator)
   - [Conditional Operator](#conditional-operator)
   - [Scope using if code blocks](#scope-using-if-code-blocks)
2. [Switch Case](#switch-case)
3. [Foreach Loop](#foreach-loop)
4. [For Loop](#for-loop)
5. [Do While Loop](#do-while-loop)
6. [While Loop](#while-loop)
7. [Modern C# Control Flow Features](#modern-c-control-flow-features)
8. [Common Anti-patterns to Avoid](#common-anti-patterns-to-avoid)
9. [Additional Resources](#additional-resources)

## Conditions

### if-else operator

```csharp
string color = "black";

if (color == "black")
{
  Console.WriteLine("It's black.");
}
else if (color == "white")
{
  Console.WriteLine("It's white.");
}
else {
  Console.WriteLine("It's other color.");
}
```

### Conditional Operator

```csharp
int saleAmount = 1001;
int discount = saleAmount > 1000 ? 100 : 50;
Console.WriteLine($"Discount: {discount}");
```

### Scope using if code blocks

```csharp
bool flag = true;
if (flag)
{
  int value = 10;
  Console.WriteLine($"Inside the code block: {value}");
}
// The following line gives error because value is declared inside the if code block.
// Console.WriteLine($"Outside the code block: {value}");
```

## Switch Case

```csharp
string fruit = "apple";

switch (fruit)
{
  case "apple":
    Console.WriteLine($"App will display information for apple.");
    break;
  case "banana":
    Console.WriteLine($"App will display information for banana.");
    break;
  case "cherry":
    Console.WriteLine($"App will display information for cherry.");
    break;
  default:
    Console.WriteLine($"App will not display information about any fruit.");
    break;
}
```

## Foreach Loop

```csharp
string[] names = { "Rowena", "Robin", "Bao" };

foreach (string name in names)
{
  Console.WriteLine(name); // "Rowena", "Robin", "Bao"
}
```

## For Loop

```csharp
for (int i = 0; i < 10; i++)
{
  if (i > 5) {
    break;
  }
  Console.WriteLine(i); // 1, 2, 3, 4, 5
}
```

## Do While Loop

```csharp
Random random = new Random();
int current = 0;

do
{
  current = random.Next(1, 11);
  
  if (current >= 8) {
    continue;
  }
  
  Console.WriteLine(current);
} while (current != 7);
```

## While Loop

```csharp
Random random = new Random();
int current = random.Next(1, 11);

while (current >= 3)
{
  Console.WriteLine(current);
  current = random.Next(1, 11);
}
Console.WriteLine($"Last number: {current}");
```

## Modern C# Control Flow Features
```csharp
// 1. Switch Expressions (C# 8.0+)
string GetDayType(DayOfWeek day) => day switch
{
    DayOfWeek.Monday or DayOfWeek.Tuesday or DayOfWeek.Wednesday 
        or DayOfWeek.Thursday or DayOfWeek.Friday => "Weekday",
    DayOfWeek.Saturday or DayOfWeek.Sunday => "Weekend",
    _ => throw new ArgumentException("Invalid day")
};

// 2. Pattern Matching with when
string AnalyzeNumber(object obj) => obj switch
{
    int i when i > 0 => "Positive integer",
    int i when i < 0 => "Negative integer",
    int => "Zero",
    double d when d > 0 => "Positive decimal",
    string s when !string.IsNullOrEmpty(s) => $"Non-empty string: {s}",
    null => "Null value",
    _ => "Unknown type"
};

// 3. Null-coalescing Assignment (C# 8.0+)
string name = null;
name ??= "Default Name"; // Assigns only if name is null

// 4. Range and Index in Loops
int[] numbers = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

// Loop through last 3 elements
foreach (int num in numbers[^3..])
{
    Console.WriteLine(num); // 7, 8, 9
}

// 5. Target-typed new (C# 9.0+)
List<string> items = new() {"apple", "banana", "cherry"};

foreach (string item in items)
{
    Console.WriteLine(item);
}

// 6. Enhanced Pattern Matching (C# 9.0+)
public static string GetQuadrant(Point point) => point switch
{
    (> 0, > 0) => "First quadrant",
    (< 0, > 0) => "Second quadrant", 
    (< 0, < 0) => "Third quadrant",
    (> 0, < 0) => "Fourth quadrant",
    (0, 0) => "Origin",
    (0, _) => "On X-axis",
    (_, 0) => "On Y-axis"
};

// 7. foreach with Index (C# 8.0+)
string[] fruits = {"apple", "banana", "cherry"};

foreach (var (fruit, index) in fruits.Select((f, i) => (f, i)))
{
    Console.WriteLine($"{index}: {fruit}");
}
```

## Common Anti-patterns to Avoid
```csharp
// AVOID: Deep nesting
if (condition1)
{
    if (condition2)
    {
        if (condition3)
        {
            // Deep nesting makes code hard to read
        }
    }
}

// PREFER: Early returns and guard clauses
if (!condition1) return;
if (!condition2) return;
if (!condition3) return;

// Code continues here

// AVOID: Complex conditions
if ((user.IsActive && user.HasPermission("read")) || 
    (user.IsAdmin && !user.IsLocked) || 
    (user.IsSuperUser && user.LastLogin > DateTime.Now.AddDays(-30)))
{
    // Complex condition
}

// PREFER: Extract to methods
if (CanUserRead(user))
{
    // Clear intent with method name
}

bool CanUserRead(User user) =>
    (user.IsActive && user.HasPermission("read")) ||
    (user.IsAdmin && !user.IsLocked) ||
    (user.IsSuperUser && user.LastLogin > DateTime.Now.AddDays(-30));
```

## Additional Resources

### Official Documentation
- [Microsoft C# Control Flow Documentation](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/statements/selection-statements) - Complete guide to C# control flow statements
- [if-else Statements](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/statements/selection-statements#the-if-statement) - Conditional execution
- [switch Statement](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/statements/selection-statements#the-switch-statement) - Multi-way branching
- [Iteration Statements](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/statements/iteration-statements) - Loops and repetition
- [Jump Statements](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/statements/jump-statements) - break, continue, return, goto

### Conditional Statements
- [Conditional Operator (?:)](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/conditional-operator) - Ternary operator usage
- [Boolean Logical Operators](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/boolean-logical-operators) - &&, ||, !, ^
- [Comparison Operators](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/comparison-operators) - ==, !=, <, >, <=, >=
- [Pattern Matching](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/functional/pattern-matching) - Modern conditional patterns

### Modern Switch Features
- [Switch Expressions](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/switch-expression) - C# 8.0+ switch expressions
- [Pattern Matching in Switch](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/patterns) - Advanced pattern matching
- [When Guards](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/when) - Conditional pattern matching
- [Tuple Patterns](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/functional/deconstruct#tuple-elements) - Matching multiple values

### Advanced Control Flow
- [Exception Handling](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/exceptions/) - try-catch-finally blocks
- [using Statements](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-statement) - Resource management
- [yield Statements](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/yield) - Iterator methods
- [goto Statement](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/statements/jump-statements#the-goto-statement) - Unconditional jumps (use sparingly)

### Collections and Iteration
- [IEnumerable<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) - Generic enumeration interface
- [ICollection<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.icollection-1) - Collection interface
- [Array Iteration](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/) - Working with arrays
- [LINQ Iteration](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/) - Functional iteration patterns

### Learning Resources
- **Books:**
  - "Clean Code" by Robert C. Martin - Writing readable control flow
  - "Code Complete" by Steve McConnell - Control structure best practices
  - "C# in Depth" by Jon Skeet - Advanced C# control flow features
  - "Effective C#" by Bill Wagner - Idiomatic control flow patterns

- **Online Courses:**
  - [Microsoft Learn C# Fundamentals](https://docs.microsoft.com/en-us/learn/paths/csharp-first-steps/) - Interactive control flow lessons
  - [Pluralsight C# Control Structures](https://www.pluralsight.com/courses/csharp-fundamentals-dev) - Professional training
  - [Coursera Programming Fundamentals](https://www.coursera.org/learn/programming-fundamentals) - Control flow concepts

### Design Patterns with Control Flow
- [Strategy Pattern](https://refactoring.guru/design-patterns/strategy/csharp/example) - Replacing complex conditionals
- [State Pattern](https://refactoring.guru/design-patterns/state/csharp/example) - State-based control flow
- [Command Pattern](https://refactoring.guru/design-patterns/command/csharp/example) - Encapsulating control flow
- [Chain of Responsibility](https://refactoring.guru/design-patterns/chain-of-responsibility/csharp/example) - Dynamic control flow

### Performance Considerations
- [Hot Paths](https://docs.microsoft.com/en-us/dotnet/core/diagnostics/debug-highcpu) - Identifying performance-critical loops
- [Profiling Tools](https://docs.microsoft.com/en-us/visualstudio/profiling/) - Measuring control flow performance

### Debugging Control Flow
- [Debugging in Visual Studio](https://docs.microsoft.com/en-us/visualstudio/debugger/) - Stepping through control flow
- [Conditional Breakpoints](https://docs.microsoft.com/en-us/visualstudio/debugger/using-breakpoints) - Breaking on specific conditions
- [Tracepoints](https://docs.microsoft.com/en-us/visualstudio/debugger/using-tracepoints) - Logging without stopping execution
- [Call Stack Analysis](https://docs.microsoft.com/en-us/visualstudio/debugger/how-to-use-the-call-stack-window) - Understanding execution flow

### Community Resources
- [Stack Overflow C# Control Flow](https://stackoverflow.com/questions/tagged/c%23+control-flow) - Q&A community
- [C# Discord](https://discord.gg/csharp) - Real-time help and discussions
- [Reddit r/csharp](https://www.reddit.com/r/csharp/) - Community discussions
- [.NET Foundation](https://dotnetfoundation.org/) - Official .NET community
- [C# Corner](https://www.c-sharpcorner.com/topics/control-flow) - Articles and tutorials

### Tools for Control Flow Analysis
- **Static Analysis:**
  - SonarQube - Code quality and complexity analysis
  - CodeMaid - Code organization and cleanup
  - ReSharper - Code analysis and refactoring
  - PVS-Studio - Static code analyzer

- **Performance Analysis:**
  - BenchmarkDotNet - Micro-benchmarking library
  - dotTrace - Performance profiler
  - PerfView - Microsoft performance toolkit
  - Application Insights - Runtime performance monitoring
```