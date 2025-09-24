# Data Types

## Table of Contents

1. [Data Types Overview](#data-types-overview)
2. [Value Types](#value-types)
   - [Integer](#integer)
     - [Minimum and Maximum Integer Size](#minimum-and-maximum-integer-size)
     - [Math Operations](#math-operations)
     - [Order of Operations](#order-of-operations)
     - [Increment and Decrement](#increment-and-decrement)
   - [Doubles](#doubles)
     - [Minimum and Maximum Double Size](#minimum-and-maximum-double-size)
   - [Decimals](#decimals)
     - [Minimum and Maximum Decimal Size](#minimum-and-maximum-decimal-size)
     - [Difference with Doubles](#difference-with-doubles)
3. [Reference Types](#reference-types)
   - [String](#string)
     - [Combine String using character escape sequences](#combine-string-using-character-escape-sequences)
     - [Combine String using string concatenation](#combine-string-using-string-concatenation)
     - [Combine String using string interpolation](#combine-string-using-string-interpolation)
   - [Array](#array)
     - [Declaration](#declaration)
     - [Assigning values](#assigning-values)
     - [Size of the array](#size-of-the-array)
4. [Other Types](#other-types)
   - [Tuples](#tuples)
   - [Generics](#generics)
   - [Anonymous Types](#anonymous-types)
5. [Advanced Type Concepts](#advanced-type-concepts)
6. [Memory and Performance Considerations](#memory-and-performance-considerations)
7. [Modern C# Type Features](#modern-c-type-features)
8. [Type Safety and Validation](#type-safety-and-validation)
9. [Performance Benchmarking](#performance-benchmarking)
10. [Additional Resources](#additional-resources)

## Data Types Overview

There are two different data types in C#:

- **Value Types:** Directly store the data. Once you assign a value, it holds that data
  - `int`, `char`, `float` are just a few examples.
- **Reference Types:** Store a memory address. They point to the address of the value.
  - `string`, `class`, `array` are commonly used.

## Value Types

### Integer

#### Minimum and Maximum Integer Size

```csharp
int max = int.MaxValue;
int min = int.MinValue;
Console.WriteLine($"The range of integers is {min} to {max}");
```

#### Math Operations

```csharp
int sum = 7 + 5;
int difference = 7 - 5;
int product = 7 * 5;
int quotient = 7 / 5;
int modulus = 7 % 5;

Console.WriteLine("Sum: " + sum);
Console.WriteLine("Difference: " + difference);
Console.WriteLine("Product: " + product);
Console.WriteLine("Quotient: " + quotient);
Console.WriteLine($"Modulus: {7 % 5}");
```

#### Order of Operations

In math, PEMDAS is an acronym that helps students remember the order of operations. The order is:

1. Parentheses (whatever is inside the parenthesis is performed first)
2. Exponents
3. Multiplication and Division (from left to right)
4. Addition and Subtraction (from left to right)

#### Increment and Decrement

```csharp
int value = 1;

value = value + 1;
Console.WriteLine("First increment: " + value);

value += 1;
Console.WriteLine("Second increment: " + value);

value++;
Console.WriteLine("Third increment: " + value); 

value = value - 1;
Console.WriteLine("First decrement: " + value);

value -= 1;
Console.WriteLine("Second decrement: " + value);

value--;
Console.WriteLine("Third decrement: " + value);
```

### Doubles 

#### Minimum and Maximum Double Size

```csharp
double max = double.MaxValue;
double min = double.MinValue;
Console.WriteLine($"The range of double is {min} to {max}");
```

### Decimals

#### Minimum and Maximum Decimal Size

```csharp
decimal min = decimal.MinValue;
decimal max = decimal.MaxValue;
Console.WriteLine($"The range of the decimal type is {min} to {max}");
```

#### Difference with Doubles

```csharp
double a = 1.0;
double b = 3.0;
Console.WriteLine(a / b);

decimal c = 1.0M;
decimal d = 3.0M;
Console.WriteLine(c / d);
```

## Reference Types

### String

#### Combine String using character escape sequences

```csharp
// Character escape sequences
Console.WriteLine("Hello\nWorld!");
Console.WriteLine("Hello\tWorld!");
Console.WriteLine("Hello \"World\"!");
Console.WriteLine("c:\\source\\repos");

// Verbatim string literal
Console.WriteLine(@"    c:\source\repos    
        (this is where your code goes)");

// Unicode escape character
Console.WriteLine("\u3053\u3093\u306B\u3061\u306F World!"); 
```

#### Combine String using string concatenation

```csharp
string firstName = "Bob";
string greeting = "Hello";
string message = greeting + " " + firstName + "!";
Console.WriteLine(message);
```

#### Combine String using string interpolation

```csharp
string firstName = "Bob";
string greeting = "Hello";
Console.WriteLine($"{greeting} {firstName}!");

// Combine verbatim literals and string interpolation
string projectName = "First-Project";
Console.WriteLine($@"C:\Output\{projectName}\Data");
```

### Array

#### Declaration

```csharp
string[] customerIds1 = new string[3];
string[] customerIds2 = [ "A123", "B456", "C789" ]; // Introduced in C#12
string[] customerIds3 = { "A123", "B456", "C789" }; // Older version
```

#### Assigning values

```csharp
string[] customerIds = new string[3];

customerIds[0] = "C123";
customerIds[1] = "C456";
customerIds[2] = "C789";
```

#### Size of the array

```csharp
string[] customerIds = [ "A123", "B456", "C789" ];
Console.WriteLine($"There are {customerIds.Length} customers.");
```

## Other Types

### Tuples

```csharp
var point = (X: 1, Y: 2);

var slope = (double)point.Y / (double)point.X;
Console.WriteLine($"A line from the origin to the point {point} has a slope of {slope}.");
```

### Generics

Generics introduces the concept of type parameters to .NET. Generics make it possible to design classes and methods that defer the specification of one or more type parameters until you use the class or method in your code. 

```csharp
// Declare the generic class.
public class GenericList<T>
{
  public void Add(T item) { 
    // Implementation for adding an item to the list
    Console.WriteLine($"Item of type {typeof(T)} added: {item}");
  }
}

public class ExampleClass { }

// Create a list of type int.
GenericList<int> list1 = new();
list1.Add(1);

// Create a list of type string.
GenericList<string> list2 = new();
list2.Add("");

// Create a list of type ExampleClass.
GenericList<ExampleClass> list3 = new();
list3.Add(new ExampleClass());
```

### Anonymous Types

Anonymous types provide a convenient way to encapsulate a set of read-only properties into a single object without having to explicitly define a type first. The type name is generated by the compiler and is not available at the source code level. The type of each property is inferred by the compiler.

```csharp
var v = new { Amount = 108, Message = "Hello" };

// Rest the mouse pointer over v.Amount and v.Message in the following
// statement to verify that their inferred types are int and string.
Console.WriteLine(v.Amount);
Console.WriteLine(v.Message);
```

## Advanced Type Concepts

```csharp
// Nullable Value Types (C# 2.0+)
int? nullableInt = null;
double? nullableDouble = 3.14;
bool hasValue = nullableInt.HasValue;
int value = nullableInt ?? 0; // Null coalescing operator

// Nullable Reference Types (C# 8.0+)
#nullable enable
string? nullableString = null;
string nonNullableString = "Hello";
// nonNullableString = null; // Warning: Converting null literal or possible null value

// var keyword for type inference
var inferredInt = 42;        // int
var inferredString = "text"; // string
var inferredArray = new[] { 1, 2, 3 }; // int[]

// Dynamic types (runtime type checking)
dynamic dynamicVar = 42;
dynamicVar = "Now I'm a string";
dynamicVar = new List<int> { 1, 2, 3 };

// Object type (base of all types)
object obj1 = 42;
object obj2 = "Hello";
object obj3 = new DateTime();

// Constants and readonly fields
const int MaxItems = 100;           // Compile-time constant
static readonly DateTime StartTime = DateTime.Now; // Runtime constant

// Default values for types
int defaultInt = default(int);       // 0
string defaultString = default(string); // null
DateTime defaultDate = default(DateTime); // 1/1/0001 12:00:00 AM

// Type checking and pattern matching
object someValue = 42;
if (someValue is int intValue)
{
    Console.WriteLine($"It's an integer: {intValue}");
}

// Switch expressions with type patterns (C# 8+)
string GetTypeInfo(object obj) => obj switch
{
    int i when i > 0 => $"Positive integer: {i}",
    int i when i < 0 => $"Negative integer: {i}",
    int => "Zero",
    string s when s.Length > 0 => $"Non-empty string: {s}",
    string => "Empty string",
    double d => $"Double: {d}",
    null => "Null value",
    _ => $"Unknown type: {obj.GetType()}"
};

// Record types (C# 9+)
public record Person(string FirstName, string LastName, int Age);
public record struct Point(double X, double Y);

// Init-only properties (C# 9+)
public class Employee
{
    public string Name { get; init; }
    public int Id { get; init; }
    public DateTime HireDate { get; init; } = DateTime.Now;
}

var employee = new Employee { Name = "John", Id = 123 };
// employee.Name = "Jane"; // Error: Init-only property

// Required members (C# 11+)
public class Product
{
    public required string Name { get; init; }
    public required decimal Price { get; init; }
    public string? Description { get; init; }
}

var product = new Product { Name = "Widget", Price = 9.99m }; // Description is optional
```

## Memory and Performance Considerations
```csharp
// Struct vs Class performance comparison
public struct PointStruct
{
    public int X { get; set; }
    public int Y { get; set; }
    
    public PointStruct(int x, int y)
    {
        X = x;
        Y = y;
    }
}

public class PointClass
{
    public int X { get; set; }
    public int Y { get; set; }
    
    public PointClass(int x, int y)
    {
        X = x;
        Y = y;
    }
}

// Value type (stack allocated, passed by value)
PointStruct structPoint1 = new PointStruct(1, 2);
PointStruct structPoint2 = structPoint1; // Copy
structPoint2.X = 3; // structPoint1.X remains 1

// Reference type (heap allocated, passed by reference)
PointClass classPoint1 = new PointClass(1, 2);
PointClass classPoint2 = classPoint1; // Same reference
classPoint2.X = 3; // classPoint1.X is also 3

// Ref struct (C# 7.2+) - stack-only structs
public ref struct StackOnlyStruct
{
    public Span<int> Data { get; }
    
    public StackOnlyStruct(Span<int> data)
    {
        Data = data;
    }
}

// Boxing and unboxing performance impact
int valueType = 42;
object boxed = valueType;        // Boxing - allocates on heap
int unboxed = (int)boxed;        // Unboxing - extracts value

// Generic collections avoid boxing
List<int> genericList = new List<int> { 1, 2, 3 }; // No boxing
ArrayList nonGenericList = new ArrayList { 1, 2, 3 }; // Boxing occurs

// Span<T> and Memory<T> for high-performance scenarios
int[] array = { 1, 2, 3, 4, 5 };
Span<int> span = array.AsSpan(1, 3); // View of elements 1, 2, 3
Memory<int> memory = array.AsMemory(2, 2); // View of elements 3, 4

// readonly struct for immutability and performance
public readonly struct ImmutablePoint
{
    public readonly int X;
    public readonly int Y;
    
    public ImmutablePoint(int x, int y)
    {
        X = x;
        Y = y;
    }
}
```

## Modern C# Type Features
```csharp
// File-scoped namespace (C# 10+)
namespace MyProject.DataTypes;

// Global using statements (C# 10+)
global using System;
global using System.Collections.Generic;
global using System.Linq;

// Top-level programs (C# 9+)
Console.WriteLine("Hello World!"); // No Main method needed

// Target-typed new expressions (C# 9+)
List<string> names = new() { "Alice", "Bob" }; // Inferred List<string>
Dictionary<int, string> dict = new(); // Inferred Dictionary<int, string>

// Pattern matching enhancements (C# 9+)
object value = 42;
string result = value switch
{
    int and (> 0 and < 100) => "Small positive number",
    int and (>= 100) => "Large positive number",
    int and (< 0) => "Negative number",
    string { Length: > 0 } => "Non-empty string",
    string => "Empty string",
    not null => "Some other non-null value",
    null => "Null value"
};

// Relational patterns (C# 9+)
string Classify(int value) => value switch
{
    < 0 => "Negative",
    0 => "Zero",
    > 0 and < 10 => "Small positive",
    >= 10 and <= 100 => "Medium positive",
    > 100 => "Large positive"
};

// Property patterns (C# 8+)
string GetPersonInfo(Person person) => person switch
{
    { Age: < 18 } => "Minor",
    { Age: >= 18, Age < 65 } => "Adult",
    { Age: >= 65 } => "Senior",
    _ => "Unknown"
};

// Tuple patterns (C# 8+)
string GetQuadrant(int x, int y) => (x, y) switch
{
    (> 0, > 0) => "First quadrant",
    (< 0, > 0) => "Second quadrant",
    (< 0, < 0) => "Third quadrant",
    (> 0, < 0) => "Fourth quadrant",
    (0, 0) => "Origin",
    (0, _) => "On X-axis",
    (_, 0) => "On Y-axis"
};

// Generic math (C# 11+)
public static T Add<T>(T left, T right) where T : INumber<T>
{
    return left + right;
}

// Static abstract members in interfaces (C# 11+)
public interface IAddable<TSelf> where TSelf : IAddable<TSelf>
{
    static abstract TSelf Zero { get; }
    static abstract TSelf operator +(TSelf left, TSelf right);
}
```

## Type Safety and Validation
```csharp
// Parameter validation
public class Calculator
{
    public double Divide(double dividend, double divisor)
    {
        if (double.IsNaN(dividend) || double.IsNaN(divisor))
            throw new ArgumentException("NaN values are not allowed");
            
        if (Math.Abs(divisor) < double.Epsilon)
            throw new ArgumentException("Cannot divide by zero");
            
        return dividend / divisor;
    }
    
    public int Factorial(int n)
    {
        ArgumentOutOfRangeException.ThrowIfNegative(n); // C# 11+
        
        if (n == 0 || n == 1) return 1;
        return n * Factorial(n - 1);
    }
}

// Type guards and validation
public static class TypeGuards
{
    public static bool IsPositiveInteger(object value)
    {
        return value is int i && i > 0;
    }
    
    public static bool IsValidEmail(string email)
    {
        return !string.IsNullOrWhiteSpace(email) && email.Contains('@');
    }
    
    public static T EnsureNotNull<T>(T? value, string paramName) where T : class
    {
        return value ?? throw new ArgumentNullException(paramName);
    }
    
    public static T EnsureNotNull<T>(T? value, string paramName) where T : struct
    {
        return value ?? throw new ArgumentNullException(paramName);
    }
}

// Custom validation attributes
[AttributeUsage(AttributeTargets.Property)]
public class RangeAttribute : Attribute
{
    public double Min { get; }
    public double Max { get; }
    
    public RangeAttribute(double min, double max)
    {
        Min = min;
        Max = max;
    }
}

public class Product
{
    public string Name { get; set; } = string.Empty;
    
    [Range(0, 10000)]
    public decimal Price { get; set; }
    
    [Range(0, 1000)]
    public int Stock { get; set; }
}
```

## Performance Benchmarking
```csharp
// BenchmarkDotNet example for type performance comparison
[MemoryDiagnoser]
[SimpleJob(RuntimeMoniker.Net60)]
public class TypePerformanceBenchmark
{
    private const int Iterations = 1000;

    [Benchmark]
    public int StructOperations()
    {
        int sum = 0;
        for (int i = 0; i < Iterations; i++)
        {
            var point = new PointStruct(i, i * 2);
            sum += point.X + point.Y;
        }
        return sum;
    }

    [Benchmark]
    public int ClassOperations()
    {
        int sum = 0;
        for (int i = 0; i < Iterations; i++)
        {
            var point = new PointClass(i, i * 2);
            sum += point.X + point.Y;
        }
        return sum;
    }

    [Benchmark]
    public double FloatingPointOperations()
    {
        double result = 0;
        for (int i = 0; i < Iterations; i++)
        {
            result += Math.Sin(i) * Math.Cos(i);
        }
        return result;
    }

    [Benchmark]
    public decimal DecimalOperations()
    {
        decimal result = 0;
        for (int i = 0; i < Iterations; i++)
        {
            result += (decimal)i / 3;
        }
        return result;
    }
}
```

## Additional Resources

### Official Documentation
- [Microsoft C# Types and Variables](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/types) - Complete type system specification
- [Built-in Types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/built-in-types) - Reference for all built-in C# types
- [Value Types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-types) - Comprehensive guide to value types
- [Reference Types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/reference-types) - Understanding reference types
- [Type System](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/) - C# type system overview

### Numeric Types Deep Dive
- [Integral Numeric Types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/integral-numeric-types) - byte, sbyte, short, ushort, int, uint, long, ulong
- [Floating-Point Types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types) - float, double, decimal
- [Numeric Conversions](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/numeric-conversions) - Implicit and explicit conversions
- [Math Class](https://docs.microsoft.com/en-us/dotnet/api/system.math) - Mathematical functions and constants

### Learning Resources
- **Books:**
  - "C# in Depth" by Jon Skeet - Advanced type system concepts
  - "Effective C#" by Bill Wagner - Best practices for C# types
  - "CLR via C#" by Jeffrey Richter - Deep dive into .NET type system
  - "C# 10 in a Nutshell" by Joseph Albahari - Comprehensive type reference

- **Online Courses:**
  - [Microsoft Learn C# Types](https://docs.microsoft.com/en-us/learn/paths/csharp-first-steps/) - Interactive tutorials
  - [Pluralsight C# Fundamentals](https://www.pluralsight.com/courses/csharp-fundamentals-dev) - Professional training

- **Practice Platforms:**
  - [LeetCode](https://leetcode.com/) - Algorithm and data structure problems
  - [HackerRank](https://www.hackerrank.com/domains/algorithms) - Programming challenges
  - [Codewars](https://www.codewars.com/kata/search/csharp) - Code kata for skill building

### Real-World Applications
- **Financial Applications:** Using decimal for monetary calculations
- **Scientific Computing:** Choosing appropriate numeric types for precision
- **Data Processing:** Working with arrays and collections efficiently
- **Configuration Management:** Using appropriate types for settings
- **API Development:** Designing DTOs with correct types
- **Database Applications:** Mapping database types to C# types
- **Game Development:** Using structs for performance-critical operations

### Community Resources
- [Stack Overflow C# Types](https://stackoverflow.com/questions/tagged/c%23+data-types) - Q&A community
- [C# Discord](https://discord.gg/csharp) - Real-time help and discussions
- [Reddit r/csharp](https://www.reddit.com/r/csharp/) - Community discussions
- [.NET Foundation](https://dotnetfoundation.org/) - Official .NET community
- [Microsoft Developer Community](https://developercommunity.visualstudio.com/) - Report issues and suggestions

### Tools and Libraries
- **NuGet Packages:**
  - [System.Memory](https://www.nuget.org/packages/System.Memory/) - High-performance memory types
  - [BenchmarkDotNet](https://www.nuget.org/packages/BenchmarkDotNet/) - Performance benchmarking
  - [FluentAssertions](https://www.nuget.org/packages/FluentAssertions/) - Better test assertions
  - [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/) - JSON serialization

- **Development Tools:**
  - Visual Studio IntelliSense for type information
  - ReSharper for advanced type analysis
  - Code analysis tools (SonarQube, CodeMaid)
  - Profiling tools (dotTrace, PerfView) for performance analysis