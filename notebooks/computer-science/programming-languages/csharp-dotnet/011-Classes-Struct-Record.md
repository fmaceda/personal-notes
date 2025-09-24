# Classes, Structs and Records

## Table of Content

1. [Class](#class)
   - [Object Initializer](#object-initializer)
2. [Struct](#struct)
   - [Difference from Classes](#difference-from-classes)
3. [Record](#record)
   - [When to Use?](#when-to-use)
4. [Interfaces](#interfaces)
5. [Abstract Classes](#abstract-classes)
   - [Example 1](#example-1)
   - [Example 2](#example-2)
   - [Example 3](#example-3)
6. [Sealed Classes](#sealed-classes)
7. [Static Classes](#static-classes)
   - [Static Members](#static-members)
8. [Partial Classes](#partial-classes)
9. [Practical Examples and Comparisons](#practical-examples-and-comparisons)
10. [Additional Resources](#additional-resources)

## Class

A type that is defined as a class is a reference type.

```csharp
//[access modifier] - [class] - [identifier]
public class Person
{
  // Properties
  public string Name { get; set; }
  public String LastName { get; set; }
  public int Age { get; set; }

  // Constructor
  public Person(string name, string lastName, int age)
  {
    Name = name;
    LastName = lastName;
    Age = age;
  }

  // Methods 
  public void Introduce()
  {
    Console.WriteLine($"Hello, my name is {Name} {LastName} and I am {Age} years old.");
  }
}

Person object1 = new Person("Fernando", "Maceda", 30); // object1 is a reference to an allocated space that will know where the object exists.
Person object2; // Reference to null

Person object3 = new Person("Cristhian", "Silva", 20); 
Person object4 = object3; // object4 has the same reference as object3. If any of both instances changes, the other one does as well. Not recommended to do that.
```

### Object Initializer

```csharp
public class Person
{
  public required string LastName { get; set; }
  public required string FirstName { get; set; }
}

// Error! Required properties not set
// var p1 = new Person();

// Correct! Required properties set
var p2 = new Person() { FirstName = "Grace", LastName = "Hopper" };
```

## Struct

### Difference from Classes

Because classes are reference types, a variable of a class object holds a reference to the address of the object on the managed heap. If a second variable of the same type is assigned to the first variable, then both variables refer to the object at that address.

On the other hand, structs are value types, a variable of a struct object holds a copy of the entire object. Instances of structs can also be created by using the new operator, but this isn't required.

```csharp
public struct Person
{
  public string Name;
  public int Age;
  public Person(string name, int age)
  {
    Name = name;
    Age = age;
  }
}

// Create  struct instance and initialize by using "new".
// Memory is allocated on thread stack.
Person p1 = new Person("Alex", 9);
Console.WriteLine($"p1 Name = {p1.Name} Age = {p1.Age}");

// Create  new struct object. Note that  struct can be initialized
// without using "new".
Person p2 = p1;

// Assign values to p2 members.
p2.Name = "Spencer";
p2.Age = 7;
Console.WriteLine($"p2 Name = {p2.Name} Age = {p2.Age}");

// p1 values remain unchanged because p2 is  copy.
Console.WriteLine($"p1 Name = {p1.Name} Age = {p1.Age}");
```

## Record

A record in C# is a class or struct that provides special syntax and behavior for working with data models. The record modifier instructs the compiler to synthesize members that are useful for types whose primary role is storing data. These members include an overload of ToString() and members that support value equality.

```csharp
// Value type: Stores all values
public record Point(int X, int Y)
{
  public double Slope() => (double)Y / (double)X;
}

Point pt1 = new Point(1, 1);
var pt2 = pt1 with { Y = 10 };
double slope = pt1.Slope();

Console.WriteLine($"The two points are {pt1} and {pt2}"); 

Console.WriteLine($"The slope of {pt1} is {slope}");
```

### When to Use?

Consider using a record in place of a class or struct in the following scenarios:

- You want to define a data model that depends on value equality.
- You want to define a type for which objects are immutable.

## Interfaces

An interface contains definitions for a group of related functionalities that a non-abstract class or a struct must implement.An interface contains definitions for a group of related functionalities that a non-abstract class or a struct must implement.

```csharp
interface IEquatable<T>
{
  bool Equals(T obj);
}

public class Car : IEquatable<Car>
{
  public string? Make { get; set; }
  public string? Model { get; set; }
  public string? Year { get; set; }

  // Implementation of IEquatable<T> interface
  public bool Equals(Car? car)
  {
    return (this.Make, this.Model, this.Year) ==
      (car?.Make, car?.Model, car?.Year);
  }
}
```

## Abstract Classes

The abstract modifier indicates that the thing being modified has a missing or incomplete implementation. An abstract class cannot be instantiated. The purpose of an abstract class is to provide a common definition of a base class that multiple derived classes can share.

### Example 1

```csharp
abstract class Shape
{
  public abstract int GetArea();
}

class Square : Shape
{
  private int _side;

  public Square(int n) => _side = n;

  // GetArea method is required to avoid a compile-time error.
  public override int GetArea() => _side * _side;
}

var sq = new Square(12);
Console.WriteLine($"Area of the square = {sq.GetArea()}");
```

### Example 2

```csharp
abstract class BaseClass
{
  protected int _x = 100;
  protected int _y = 150;

  // Abstract method
  public abstract void AbstractMethod();

  // Abstract properties
  public abstract int X { get; }
  public abstract int Y { get; }
}

class DerivedClass : BaseClass
{
  public override void AbstractMethod()
  {
    _x++;
    _y++;
  }

  public override int X   // overriding property
  {
    get
    {
      return _x + 10;
    }
  }

  public override int Y   // overriding property
  {
    get
    {
      return _y + 10;
    }
  }
}

var o = new DerivedClass();
o.AbstractMethod();
Console.WriteLine($"x = {o.X}, y = {o.Y}");
```

### Example 3

```csharp
public abstract class Shape
{
  public string Color { get; set; }

  // Constructor of the abstract class
  protected Shape(string color)
  {
    Color = color;
    Console.WriteLine($"Created a shape with color {color}.");
  }

  // Abstract method that must be implemented by derived classes
  public abstract double CalculateArea();
}

public class Square : Shape
{
  public double Side { get; set; }

  // Constructor of the derived class calling the base class constructor
  public Square(string color, double side) : base(color)
  {
    Side = side;
  }

  public override double CalculateArea()
  {
    return Side * Side;
  }
}

Square square = new Square("red", 5);
Console.WriteLine($"Area of the square: {square.CalculateArea()}");
```

## Sealed Classes

A sealed class cannot be used as a base class.  Because they can never be used as a base class, some run-time optimizations can make calling sealed class members slightly faster.

```csharp
public class A
{
  public virtual void DoWork()
  {
    Console.WriteLine("Doing work in A");
  }
}

public class B : A
{
  public sealed override void DoWork()
  {
    Console.WriteLine("Doing work in B");
  }
}

public sealed class C : B
{
  // Cannot override DoWork here because B's DoWork is sealed.
  // public override void DoWork() { } // This would cause a compile error.
}
```

## Static Classes

A static class is basically the same as a non-static class, but there's one difference: a static class can't be instantiated. In other words, you can't use the new operator to create a variable of the class type. Because there's no instance variable, you access the members of a static class by using the class name itself. 

```csharp
public static class TemperatureConverter
{
  public static double CelsiusToFahrenheit(double celsius)
  {
    // Convert Celsius to Fahrenheit.
    double fahrenheit = (celsius * 9 / 5) + 32;

    return fahrenheit;
  }
}

double celsius = 25.0;
double fahrenheit = TemperatureConverter.CelsiusToFahrenheit(celsius);
Console.WriteLine($"{celsius}°C is {fahrenheit}°F");
```

### Static Members

A non-static class can contain static methods, fields, properties, or events. The static member is callable on a class even when no instance of the class exists. The static member is always accessed by the class name, not the instance name. Only one copy of a static member exists, regardless of how many instances of the class are created. Static methods and properties can't access non-static fields and events in their containing type, and they can't access an instance variable of any object unless it's explicitly passed in a method parameter.

```csharp
public class Automobile
{
  public static int NumberOfWheels = 4;

  public static int SizeOfGasTank
  {
    get
    {
      return 15;
    }
  }

  public static void Drive() { }

  // Other non-static fields and properties...
}

Automobile.Drive();
int i = Automobile.NumberOfWheels;
```

## Partial Classes

It's possible to split the definition of a class, a struct, an interface, or a member over two or more source files. Each source file contains a section of the type or member definition, and all parts are combined when the application is compiled.

```csharp
// This is in Employee_Part1.cs
public partial class Employee
{
  public void DoWork()
  {
    Console.WriteLine("Employee is working.");
  }
}

// This is in Employee_Part2.cs
public partial class Employee
{
  public void GoToLunch()
  {
    Console.WriteLine("Employee is at lunch.");
  }
}

//Main program demonstrating the Employee class usage
Employee emp = new Employee();
emp.DoWork();
emp.GoToLunch();
```

## Practical Examples and Comparisons
```csharp
// Quick reference for type selection

// Use CLASS when:
// - You need reference semantics
// - Object identity is important
// - Inheritance is required
// - Large objects (> 16 bytes typically)
public class Person
{
    public string Name { get; set; }
    public DateTime BirthDate { get; set; }
    public List<string> Hobbies { get; set; } = new();
}

// Use STRUCT when:
// - You need value semantics
// - Small, simple data (< 16 bytes recommended)
// - Immutable data
// - No inheritance needed
public readonly struct Point
{
    public double X { get; init; }
    public double Y { get; init; }
    
    public Point(double x, double y) => (X, Y) = (x, y);
    
    public double DistanceFrom(Point other) =>
        Math.Sqrt(Math.Pow(X - other.X, 2) + Math.Pow(Y - other.Y, 2));
}

// Use RECORD when:
// - Data-focused types
// - Value equality semantics
// - Immutable data models
// - Easy copying with modifications
public record Customer(int Id, string Name, string Email)
{
    public record Address(string Street, string City, string PostalCode);
    public Address? BillingAddress { get; init; }
}

// Use INTERFACE when:
// - Defining contracts
// - Multiple inheritance of type
// - Loose coupling
// - Dependency injection
public interface IPaymentProcessor
{
    Task<PaymentResult> ProcessPaymentAsync(decimal amount, PaymentMethod method);
}

// Use ABSTRACT CLASS when:
// - Partial implementation
// - Shared code among derived classes
// - Common fields/properties needed
// - Template method pattern
public abstract class PaymentProcessorBase
{
    protected abstract Task<bool> ValidatePaymentAsync(decimal amount);
    
    public async Task<PaymentResult> ProcessAsync(decimal amount)
    {
        if (!await ValidatePaymentAsync(amount))
            return PaymentResult.Invalid;
            
        // Common processing logic here
        return PaymentResult.Success;
    }
}
```

## Additional Resources

### Official Documentation
- [Microsoft C# Classes Documentation](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/types/classes) - Comprehensive guide to C# classes
- [Structs](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/struct) - Understanding value types and structs
- [Records](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/record) - Modern data-focused types in C#
- [Interfaces](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/types/interfaces) - Contracts and interface implementation
- [Abstract Classes and Members](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/abstract-and-sealed-classes-and-class-members) - Base classes and abstract members

### Type System Fundamentals
- [Value Types vs Reference Types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-types) - Understanding memory allocation differences
- [Object-Oriented Programming](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/) - Core OOP concepts in C#

### Advanced Type Features
- [Generics](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/generics/) - Type-safe generic programming
- [Extension Methods](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods) - Adding methods to existing types
- [Anonymous Types](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/types/anonymous-types) - Compiler-generated types
- [Nullable Reference Types](https://docs.microsoft.com/en-us/dotnet/csharp/nullable-references) - Handling null values safely

### Memory and Performance
- [Memory Management](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/) - Understanding heap vs stack allocation
- [Performance Considerations](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/choosing-between-class-and-struct) - When to use classes vs structs
- [Boxing and Unboxing](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/boxing-and-unboxing) - Value type conversions

### Design Patterns and Best Practices
- [SOLID Principles](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles) - Object-oriented design principles
- [Immutable Types](https://docs.microsoft.com/en-us/dotnet/api/system.collections.immutable) - Creating unchangeable objects
- [Factory Pattern](https://refactoring.guru/design-patterns/factory-method/csharp/example) - Object creation patterns

### Modern C# Features
- [Primary Constructors](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-12#primary-constructors) - C# 12 constructor syntax
- [Required Members](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/required) - Enforcing property initialization
- [Init-only Properties](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/init) - Immutable property setters
- [With Expressions](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/with-expression) - Non-destructive mutation

### Learning Resources
- **Books:**
  - "C# in Depth" by Jon Skeet - Deep understanding of C# type system
  - "Effective C#" by Bill Wagner - Best practices and idioms
  - "Clean Code" by Robert C. Martin - Writing maintainable code
  - "Head First Design Patterns" by Freeman & Robson - Understanding design patterns

- **Online Courses:**
  - [Microsoft Learn C# Path](https://docs.microsoft.com/en-us/learn/paths/csharp-first-steps/) - Interactive learning
  - [Pluralsight C# Fundamentals](https://www.pluralsight.com/courses/csharp-fundamentals-dev) - Professional training

### Tools and Development Environment
- **IDEs and Editors:**
  - Visual Studio - Full-featured development environment
  - Visual Studio Code - Lightweight cross-platform editor
  - JetBrains Rider - Advanced .NET IDE
  - LINQPad - Interactive C# scratchpad

- **Code Analysis:**
  - [Code Analysis](https://docs.microsoft.com/en-us/dotnet/fundamentals/code-analysis/overview) - Static code analysis
  - [SonarQube](https://www.sonarqube.org/) - Code quality analysis
  - [StyleCop](https://github.com/DotNetAnalyzers/StyleCopAnalyzers) - Code style enforcement

### Community and Support
- [Stack Overflow C# Tag](https://stackoverflow.com/questions/tagged/c%23) - Q&A community
- [C# Corner](https://www.c-sharpcorner.com/) - Articles and tutorials
- [Reddit r/csharp](https://www.reddit.com/r/csharp/) - Community discussions
- [.NET Foundation](https://dotnetfoundation.org/) - Official .NET community
- [C# Discord Server](https://discord.gg/csharp) - Real-time help and discussions

### Performance and Benchmarking
- [BenchmarkDotNet](https://benchmarkdotnet.org/) - .NET benchmarking library
- [Performance Tips](https://docs.microsoft.com/en-us/dotnet/standard/collections/when-to-use-generic-collections) - Choosing right collection types
- [Memory Profiling](https://docs.microsoft.com/en-us/visualstudio/profiling/) - Understanding memory usage patterns