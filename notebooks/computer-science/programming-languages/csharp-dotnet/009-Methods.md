# Methods

## Table of Content

1. [Overview](#overview)
2. [Using Parameters](#using-parameters)
3. [Optional Parameters](#optional-parameters)
4. [Returning Values](#returning-values)
5. [Stateless Method](#stateless-method)
6. [Stateful Method](#stateful-method)
7. [Extension Methods](#extension-methods)
   - [Declaration](#declaration)
   - [Value and Reference Types](#value-and-reference-types)
8. [Passing by reference vs. passing by value](#passing-by-reference-vs-passing-by-value)
9. [Async Methods](#async-methods)
10. [Expression body definitions](#expression-body-definitions)
11. [Named arguments](#named-arguments)
12. [Common Method Patterns and Examples](#common-method-patterns-and-examples)
13. [Additional Resources](#additional-resources)

## Overview

```csharp
Console.WriteLine("Generating random numbers:");
DisplayRandomNumbers(); // 17 29 46 36 3 

void DisplayRandomNumbers() 
{
    Random random = new Random();

    for (int i = 0; i < 5; i++) 
    {
        Console.Write($"{random.Next(1, 100)} ");
    }

    Console.WriteLine();
}
```

## Using Parameters

```csharp
CountTo(5);

void CountTo(int max) 
{
  for (int i = 0; i < max; i++)
  {
    Console.Write($"{i}, "); // 0, 1, 2, 3, 4
  }
}
```

## Optional Parameters

```csharp
CountTo();

void CountTo(int max = 10) 
{
  for (int i = 0; i < max; i++)
  {
    Console.Write($"{i}, "); // 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
  }
}
```

## Returning Values

```csharp
int sum = SumTo(5);
Console.Write($"sum: {sum}"); // sum: 15

int SumTo(int max) 
{
  int result = 0;

  for (int i = 1; i <= max; i++)
  {
    result += i;
  }

  return result;
}
```

## Stateless Method

The following code is stateless because it doesn't require to store any state to work, you just call the static method WriteLine from Console class.

```csharp
Console.WriteLine("Hello World!");
```

## Stateful Method

```csharp
Random dice = new Random();
int roll = dice.Next(1, 7);
```

## Extension Methods

Extension members enable you to "add" methods to existing types without creating a new derived type, recompiling, or otherwise modifying the original type.

Extension methods are static methods, but they're called as if they were instance methods on the extended type. 

### Declaration

Beginning with C# 14, you can declare extension blocks.

```csharp
namespace CustomExtensionMembers;

public static class MyExtensions
{
  extension(string str)
  {
    public int WordCount() =>
      str.Split([' ', '.', '?'], StringSplitOptions.RemoveEmptyEntries).Length;
  }
}
```

Before C# 14, you declare an extension method by adding the this modifier to the first parameter.

```csharp
namespace CustomExtensionMethods;

public static class MyExtensions
{
  public static int WordCount(this string str) =>
    str.Split([' ', '.', '?'], StringSplitOptions.RemoveEmptyEntries).Length;
}
```

And it can be called from an application by using the syntax for accessing instance members:

```csharp
string s = "Hello Extension Methods";
int i = s.WordCount();
```

### Value and Reference Types

```csharp
public static class IntExtensions
{
  extension(int number)
  {
    public void Increment()
      => number++;
  }

  // Take note of the extra ref keyword here
  extension(ref int number)
  {
    public void RefIncrement()
      => number++;
  }
}
```

Different extension blocks are required to distinguish by-value and by-ref parameter modes for the receiver.

```csharp
int x = 1;

// Takes x by value leading to the extension method
// Increment modifying its own copy, leaving x unchanged
x.Increment();
Console.WriteLine($"x is now {x}"); // x is now 1

// Takes x by reference leading to the extension method
// RefIncrement changing the value of x directly
x.RefIncrement();
Console.WriteLine($"x is now {x}"); // x is now 2
```

## Passing by reference vs. passing by value

By default, when an instance of a value type is passed to a method, its copy is passed instead of the instance itself. Therefore, changes to the argument have no effect on the original instance in the calling method. To pass a value-type instance by reference, use the ref keyword.

```csharp
int valA = 5;
int valB = 5;
int valC;
int valD = 5;

Console.WriteLine($"Before ChangeValue: valA = {valA}, valB = {valB}");

ChangeValue(valA);
ChangeValueRef(ref valB);

Console.WriteLine($"After ChangeValue: valA = {valA}, valB = {valB}");

ChangeValueOut(out valC);
ChangeValueIn(in valD);

Console.WriteLine($"After ChangeValueOut: valC = {valC}");
Console.WriteLine($"After ChangeValueIn: valD = {valD}");

int initial = 10;
SumParams(initial, 1, 2, 3, 4, 5); // "1, 2, 3, 4, 5" is a params array.

public void ChangeValue(int value) 
{
  value = 10; // This change won't affect the original variable
}

public void ChangeValueRef(ref int value) 
{
  value = 10; // This change will affect the original variable
}

public void ChangeValueOut(out int value) 
{
  value = 10; // This will initialize the variable
}

public void ChangeValueIn(in int value) 
{
  // value = 10; // This will cause a compile-time error because 'in' parameters are read-only
}

public void SumParams(int initial, params int[] numbers) 
{
  int sum = initial;

  foreach (var number in numbers) 
  {
    sum += number;
  }
  Console.WriteLine($"Sum: {sum}");
}
```

## Async Methods

By using the async feature, you can invoke asynchronous methods without using explicit callbacks or manually splitting your code across multiple methods or lambda expressions.

If you mark a method with the async modifier, you can use the await operator in the method. When control reaches an await expression in the async method, control returns to the caller, and progress in the method is suspended until the awaited task completes. When the task is complete, execution can resume in the method.

An async method typically has a return type of Task &lt;TResult&gt;, Task, IAsyncEnumerable &lt;T&gt; or void. The void return type is used primarily to define event handlers, where a void return type is required. An async method that returns void can't be awaited, and the caller of a void-returning method can't catch exceptions that the method throws. An async method can have any task-like return type.

```csharp
await DoSomethingAsync();

public static async Task DoSomethingAsync()
{
  Task<int> delayTask = DelayAsync();
  int result = await delayTask;

  // The previous two statements may be combined into
  // the following statement.
  //int result = await DelayAsync();

  Console.WriteLine($"Result: {result}");
}

public static async Task<int> DelayAsync()
{
  await Task.Delay(100);
  return 5;
}

// Example output:
//   Result: 5
```

## Expression body definitions

It is common to have method definitions that simply return immediately with the result of an expression, or that have a single statement as the body of the method. There is a syntax shortcut for defining such methods using =>

```csharp
public class Point
{
  public int x { get; set; }
  public int y { get; set; }

  public Point(int x, int y)
  {
    this.x = x;
    this.y = y;
  }

  public Point Move(int dx, int dy) => new Point(x + dx, y + dy);
}
```

## Named arguments

Named arguments free you from matching the order of arguments to the order of parameters in the parameter lists of called methods.

```csharp
// The method can be called in the normal way, by using positional arguments.
PrintOrderDetails("Gift Shop", 31, "Red Mug");

// Named arguments can be supplied for the parameters in any order.
PrintOrderDetails(orderNum: 31, productName: "Red Mug", sellerName: "Gift Shop");
PrintOrderDetails(productName: "Red Mug", sellerName: "Gift Shop", orderNum: 31);

// Named arguments mixed with positional arguments are valid
// as long as they are used in their correct position.
PrintOrderDetails("Gift Shop", 31, productName: "Red Mug");
PrintOrderDetails(sellerName: "Gift Shop", 31, productName: "Red Mug"); 
PrintOrderDetails("Gift Shop", orderNum: 31, "Red Mug");

static void PrintOrderDetails(string sellerName, int orderNum, string productName)
{
  if (string.IsNullOrWhiteSpace(sellerName))
  {
      throw new ArgumentException(message: "Seller name cannot be null or empty.", paramName: nameof(sellerName));
  }

  Console.WriteLine($"Seller: {sellerName}, Order #: {orderNum}, Product: {productName}");
}
```

## Common Method Patterns and Examples
```csharp
// 1. Factory Methods
public static class UserFactory
{
    public static User CreateAdmin(string name) => new User(name, Role.Admin);
    public static User CreateGuest(string name) => new User(name, Role.Guest);
}

// 2. Builder Pattern Methods
public class QueryBuilder
{
    public QueryBuilder Select(string columns)  { /* implementation */ return this; }
    public QueryBuilder From(string table)      { /* implementation */ return this; }
    public QueryBuilder Where(string condition) { /* implementation */ return this; }
    public string Build() => /* build query */;
}

// 3. Fluent Interface Methods
public class StringBuilder
{
    public StringBuilder Append(string value)    { /* implementation */ return this; }
    public StringBuilder AppendLine(string value) { /* implementation */ return this; }
    public StringBuilder Clear()                  { /* implementation */ return this; }
}

// 4. Generic Methods
public static T Clamp<T>(T value, T min, T max) where T : IComparable<T>
{
    if (value.CompareTo(min) < 0) return min;
    if (value.CompareTo(max) > 0) return max;
    return value;
}

// 5. Async Enumerable Methods
public static async IAsyncEnumerable<int> GenerateNumbersAsync(int count)
{
    for (int i = 0; i < count; i++)
    {
        await Task.Delay(100);
        yield return i;
    }
}

// 6. Extension Method Chains
public static class StringExtensions
{
    public static string Truncate(this string value, int maxLength) =>
        value.Length <= maxLength ? value : value[..maxLength];
    
    public static string RemoveSpaces(this string value) =>
        value.Replace(" ", "");
    
    public static string Capitalize(this string value) =>
        char.ToUpper(value[0]) + value[1..].ToLower();
}

// Usage: "hello world".RemoveSpaces().Capitalize().Truncate(8)
```

## Additional Resources

### Official Documentation
- [Microsoft C# Methods Documentation](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/methods) - Comprehensive guide to C# methods
- [Method Parameters](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/method-parameters) - Complete reference for parameter types
- [Extension Methods](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods) - Creating and using extension methods
- [Async Programming](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/) - Asynchronous programming with async/await

### Method Parameter Types
- [ref keyword](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/ref) - Passing parameters by reference
- [out keyword](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/out-parameter-modifier) - Output parameters
- [in keyword](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/in-parameter-modifier) - Read-only reference parameters
- [params keyword](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/params) - Variable number of arguments
- [Optional Parameters](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments) - Default parameter values

### Advanced Method Concepts
- [Method Overriding](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/versioning-with-the-override-and-new-keywords) - Virtual and override keywords
- [Abstract Methods](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/abstract-and-sealed-classes-and-class-members) - Methods without implementation
- [Static Methods](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/static-classes-and-static-class-members) - Class-level methods

### Modern C# Method Features
- [Local Functions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/local-functions) - Methods inside methods
- [Expression-bodied Members](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/expression-bodied-members) - Concise method syntax
- [Pattern Matching](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/functional/pattern-matching) - Advanced conditional logic
- [Primary Constructors](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-12#primary-constructors) - Constructor parameters as fields

### Asynchronous Programming
- [Task and Task<T>](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task) - Asynchronous operation representation
- [async/await Best Practices](https://docs.microsoft.com/en-us/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming) - Writing efficient async code
- [ConfigureAwait](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.configureawait) - Context switching control
- [Cancellation Tokens](https://docs.microsoft.com/en-us/dotnet/standard/threading/cancellation-in-managed-threads) - Canceling async operations
- [Parallel Programming](https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/) - Parallel execution patterns

### Performance and Best Practices
- [Method Inlining](https://docs.microsoft.com/en-us/dotnet/standard/managed-execution-process) - JIT optimization
- [Memory Allocation](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/) - Understanding method call overhead
- [Span<T> and Memory<T>](https://docs.microsoft.com/en-us/dotnet/standard/memory-and-spans/) - High-performance memory operations
- [Benchmarking Methods](https://benchmarkdotnet.org/) - Performance measurement tools

### Functional Programming Concepts
- [Lambda Expressions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) - Anonymous functions
- [Delegates and Events](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/) - Function pointers and callbacks
- [LINQ Methods](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/standard-query-operators-overview) - Functional-style data operations

### Learning Resources
- **Books:**
  - "C# in Depth" by Jon Skeet - Advanced method concepts and internals
  - "Effective C#" by Bill Wagner - Best practices for method design
  - "Clean Code" by Robert C. Martin - Writing maintainable methods
  - "Concurrency in C# Cookbook" by Stephen Cleary - Async programming patterns

- **Online Courses:**
  - [Microsoft Learn C# Path](https://docs.microsoft.com/en-us/learn/paths/csharp-first-steps/) - Interactive C# learning
  - [Pluralsight C# Methods](https://www.pluralsight.com/courses/csharp-fundamentals-dev) - Professional training

### Design Patterns with Methods
- [Template Method Pattern](https://refactoring.guru/design-patterns/template-method/csharp/example) - Defining algorithm skeleton
- [Strategy Pattern](https://refactoring.guru/design-patterns/strategy/csharp/example) - Interchangeable algorithms
- [Command Pattern](https://refactoring.guru/design-patterns/command/csharp/example) - Encapsulating method calls
- [Observer Pattern](https://refactoring.guru/design-patterns/observer/csharp/example) - Event-driven method execution

### Testing Methods
- [Unit Testing](https://docs.microsoft.com/en-us/dotnet/core/testing/) - Testing individual methods
- [Mocking](https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices) - Isolating method dependencies
- [Integration Testing](https://docs.microsoft.com/en-us/aspnet/core/test/integration-tests) - Testing method interactions

### Error Handling in Methods
- [Exception Handling](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/exceptions/) - Try-catch-finally blocks
- [Custom Exceptions](https://docs.microsoft.com/en-us/dotnet/standard/exceptions/how-to-create-user-defined-exceptions) - Creating meaningful error types

### Performance Optimization
- [Memory-Efficient Methods](https://docs.microsoft.com/en-us/dotnet/standard/memory-and-spans/) - Reducing allocations
- [Profiling Tools](https://docs.microsoft.com/en-us/visualstudio/profiling/) - Measuring method performance

### Community Resources
- [Stack Overflow C# Methods](https://stackoverflow.com/questions/tagged/c%23+methods) - Q&A community
- [C# Discord](https://discord.gg/csharp) - Real-time help and discussions
- [Reddit r/csharp](https://www.reddit.com/r/csharp/) - Community discussions
- [.NET Foundation](https://dotnetfoundation.org/) - Official .NET community
- [C# Corner](https://www.c-sharpcorner.com/topics/methods) - Articles and tutorials

### Tools for Method Development
- **IDEs:**
  - Visual Studio - Full-featured development environment
  - Visual Studio Code - Lightweight editor with C# extension
  - JetBrains Rider - Cross-platform .NET IDE

- **Analysis Tools:**
  - ReSharper - Code analysis and refactoring
  - SonarQube - Code quality analysis
  - CodeMaid - Code cleanup and organization
  - PVS-Studio - Static code analyzer
```