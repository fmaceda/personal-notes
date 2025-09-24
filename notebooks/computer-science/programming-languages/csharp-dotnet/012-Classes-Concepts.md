# Classes Concepts

## Table of Content

1. [Polymorphism](#polymorphism)
   - [Virtual and Override](#virtual-and-override)
   - [Hiding Base Class Method](#hiding-base-class-method)
2. [Members](#members)
   - [Fields](#fields)
   - [Constants](#constants)
   - [Properties](#properties)
   - [Methods](#methods)
   - [Events](#events)
   - [Operators](#operators)
     - [Operators Overloading](#operators-overloading)
   - [Indexers](#indexers)
   - [Constructors](#constructors)
     - [Static Constructors](#static-constructors)
     - [Primary Constructor](#primary-constructor)
   - [Finalizers](#finalizers)
   - [Nested Types](#nested-types)
   - [Access Modifiers for Members](#access-modifiers-for-members)
3. [Practical Examples and Exercises](#practical-examples-and-exercises)
4. [Additional Resources](#additional-resources)

## Polymorphism

The C# language is designed so that versioning between base and derived classes in different libraries can evolve and maintain backward compatibility. This means, for example, that the introduction of a new member in a base class with the same name as a member in a derived class is completely supported by C# and does not lead to unexpected behavior. It also means that a class must explicitly state whether a method is intended to override an inherited method, or whether a method is a new method that hides a similarly named inherited method.

### Virtual and Override

By default, C# methods are not virtual. If a method is declared as virtual, any class inheriting the method can implement its own version.

```csharp
public class Animal(string Name)
{
  public virtual void MakeNoise()
  {
    Console.WriteLine($"{Name} makes a noise.");
  }
}

public class Dog(string Name) : Animal(Name)
{
  public override void MakeNoise()
  {
    Console.WriteLine($"{Name} barks.");

    base.MakeNoise(); // Calls the base class method
  }
}

var dog = new Dog("Max");

dog.MakeNoise();
```

### Hiding Base Class Method

In C#, a method in a derived class can have the same name as a method in the base class. You can hide the base class methods by using the new keyword.

```csharp
class BaseClass  
{  
  public virtual void Method1()  
  {  
    Console.WriteLine("Base - Method1");  
  }

  public void Method2()  
  {  
    Console.WriteLine("Base - Method2");  
  }  
}  
  
class DerivedClass : BaseClass  
{  
  public override void Method1()  
  {  
    Console.WriteLine("Derived - Method1");  
  } 
  
  public new void Method2()  
  {  
    Console.WriteLine("Derived - Method2");  
  }  
}

BaseClass bc = new BaseClass();
DerivedClass dc = new DerivedClass();
BaseClass bcdc = new DerivedClass();

bc.Method1();  
bc.Method2();  
dc.Method1();  
dc.Method2();  
bcdc.Method1();  
bcdc.Method2();  
```

## Members

Classes and structs have members that represent their data and behavior.

| Member | Description |
| --- | --- |
| Fields | Fields are variables declared at class scope. A field may be a built-in numeric type or an instance of another class. For example, a calendar class may have a field that contains the current date. |
|Constants | Constants are fields whose value is set at compile time and cannot be changed. |
| Properties | Properties are methods on a class that are accessed as if they were fields on that class. A property can provide protection for a class field to keep it from being changed without the knowledge of the object. |
| Methods | Methods define the actions that a class can perform. Methods can take parameters that provide input data, and can return output data through parameters. Methods can also return a value directly, without using a parameter. |
| Events | Events provide notifications about occurrences, such as button clicks or the successful completion of a method, to other objects. Events are defined and triggered by using delegates. |
| Operators | Overloaded operators are considered type members. When you overload an operator, you define it as a public method in a type. For more information, see Operator overloading. |
| Indexers | Indexers enable an object to be indexed in a manner similar to arrays. |
| Constructors | Constructors are methods that are called when the object is first created. They are often used to initialize the data of an object. |
| Finalizers | Finalizers are used very rarely in C#. They are methods that are called by the runtime execution engine when the object is about to be removed from memory. They are generally used to make sure that any resources which must be released are handled appropriately. |
| Nested Types | Nested types are types declared within another type. Nested types are often used to describe objects that are used only by the types that contain them. |

### Fields

A field is a variable of any type that is declared directly in a class or struct. Fields are members of their containing type.

```csharp
public class Person {
  public string Name { get; set; }
  public int Age { get; set; }
}

var person = new Person {
  Name = "John",
  Age = 30
};
```

### Constants 

Constants are immutable values which are known at compile time and do not change for the life of the program.

```csharp
class Calendar1
{
  public const int Months = 12;
}
```

### Properties

A property is a member that provides a flexible mechanism to read, write, or compute the value of a data field. Properties appear as public data members, but they're implemented as special methods called accessors.

```csharp
public class Person
{
  private string _firstName;
  public string FirstName
  {
    get => _firstName;
    set
    {
      _firstName = value;
      _fullName = null;
    }
  }
  public required string LastName { get; set; }
  public string Age { get; init; } // 'init' allows setting during object initialization only.

  private string _fullName;
  public string FullName // Property with backing fields.
  {
    get
    {
      if (_fullName is null)
        _fullName = $"{FirstName} {LastName}";
      return _fullName;
    }
  }
}

var person = new Person
{
  LastName = "Doe", // LastName is required, so it must be set.
  Age = "30" // Age can be set during initialization.
};
```

### Methods

A method is a code block that contains a series of statements. A program causes the statements to be executed by calling the method and specifying any required method arguments. In C#, every executed instruction is performed in the context of a method.

```csharp
abstract class Motorcycle
{
  // Anyone can call this.
  public void StartEngine() {/* Method statements here */ }

  // Derived classes can override the base class implementation.
  public virtual int Drive(int miles, int speed) { /* Method statements here */ return 1; }

  // Derived classes must implement this.
  public abstract double GetTopSpeed();
}

class TestMotorcycle : Motorcycle
{
  public override double GetTopSpeed()
  {
    return 108.4;
  }
}

TestMotorcycle moto = new TestMotorcycle();

moto.StartEngine();
moto.Drive(5, 20);
double speed = moto.GetTopSpeed();
Console.WriteLine($"My top speed is {speed}");
```

### Events

Events enable a class or object to notify other classes or objects when something of interest occurs. The class that sends (or raises) the event is called the publisher and the classes that receive (or handle) the event are called subscribers.

```csharp
// Define a custom event arguments class.
public class TestClass {
  private string _value;
  public event EventHandler<ValueChangedArgs> ValueChanged;

  public void SetValue(string value) {
    if (value != _value) {
      _value = value;
      OnValueChanged(new ValueChangedArgs(value));
    }
  }

  protected void OnValueChanged(ValueChangedArgs eventArgs) {
    ValueChanged?.Invoke(this, eventArgs);
    // ValueChanged(this, eventArgs); // Other way to invoke the event.
  }
}

public class ValueChangedArgs : EventArgs
{
  public string Value { get; }

  public ValueChangedArgs(string value) => Value = value;
}

public void OnValueChanged(object sender, ValueChangedArgs eventArgs) {
  Console.WriteLine($"Value changed to: {eventArgs.Value}");
}

var test = new TestClass();

// Subscribe to the event.
test.ValueChanged += OnValueChanged;

// Unsubscribe from the event.
// test.ValueChanged -= OnValueChanged;

test.SetValue("Hello, World!");
```

### Operators

C# provides a number of operators. Many of them are supported by the built-in types and allow you to perform basic operations with values of those types.

#### Operators Overloading

```csharp
public struct Fraction
{
  private int numerator;
  private int denominator;

  public Fraction(int numerator, int denominator)
  {
    if (denominator == 0)
    {
      throw new ArgumentException("Denominator cannot be zero.", nameof(denominator));
    }
    this.numerator = numerator;
    this.denominator = denominator;
  }

  public static Fraction operator +(Fraction operand) => operand;
  public static Fraction operator -(Fraction operand) => new Fraction(-operand.numerator, operand.denominator);

  public static Fraction operator +(Fraction left, Fraction right)
    => new Fraction(left.numerator * right.denominator + right.numerator * left.denominator, left.denominator * right.denominator);

  public static Fraction operator -(Fraction left, Fraction right)
    => left + (-right);

  public static Fraction operator *(Fraction left, Fraction right)
    => new Fraction(left.numerator * right.numerator, left.denominator * right.denominator);

  public static Fraction operator /(Fraction left, Fraction right)
  {
    if (right.numerator == 0)
    {
      throw new DivideByZeroException();
    }
    return new Fraction(left.numerator * right.denominator, left.denominator * right.numerator);
  }

  // Define increment and decrement to add 1/den, rather than 1/1.
  public static Fraction operator ++(Fraction operand)
    => new Fraction(operand.numerator++, operand.denominator);

  public static Fraction operator --(Fraction operand) =>
    new Fraction(operand.numerator--, operand.denominator);

  public override string ToString() => $"{numerator} / {denominator}";

  // New operators allowed in C# 14:
  // public void operator +=(Fraction operand) =>
  //   (numerator, denominator ) =
  //   (
  //     numerator * operand.denominator + operand.numerator * denominator,
  //     denominator * operand.denominator
  //   );

  // public void operator -=(Fraction operand) =>
  //   (numerator, denominator) =
  //   (
  //     numerator * operand.denominator - operand.numerator * denominator,
  //     denominator * operand.denominator
  //   );

  // public void operator *=(Fraction operand) =>
  //   (numerator, denominator) =
  //   (
  //     numerator * operand.numerator,
  //     denominator * operand.denominator
  //   );

  // public void operator /=(Fraction operand)
  // {
  //   if (operand.numerator == 0)
  //   {
  //     throw new DivideByZeroException();
  //   }
  //   (numerator, denominator) =
  //   (
  //     numerator * operand.denominator,
  //     denominator * operand.numerator
  //   );
  // }

  // public void operator ++() => numerator++;

  // public void operator --() => numerator--;
}

var a = new Fraction(5, 4);
var b = new Fraction(1, 2);
Console.WriteLine(-a);   // output: -5 / 4
Console.WriteLine(a + b);  // output: 14 / 8
Console.WriteLine(a - b);  // output: 6 / 8
Console.WriteLine(a * b);  // output: 5 / 8
Console.WriteLine(a / b);  // output: 10 / 4
```

### Indexers

You define indexers when instances of a class or struct can be indexed like an array or other collection. The indexed value can be set or retrieved without explicitly specifying a type or instance member.

```csharp
public class SampleCollection<T>
{
  // Declare an array to store the data elements.
  private T[] arr = new T[100];

  // Define the indexer to allow client code to use [] notation.
  public T this[int index]
  {
    get => arr[index];
    set => arr[index] = value;
  }
}

var sample = new SampleCollection<string>();
sample[0] = "Custom index";

Console.WriteLine($"Index 0: {sample[0]}");
```

### Constructors

A constructor is a method called by the runtime when an instance of a class or a struct is created. A class or struct can have multiple constructors that take different arguments.

```csharp
public class Person
{
  public string LastName;
  public string FirstName;

  public Person(string lastName, string firstName)
  {
    LastName = lastName;
    FirstName= firstName;
  }
}

var person = new Person("Doe", "John");
Console.WriteLine($"Person: {person.LastName}, {person.FirstName}");
```

#### Static Constructors

Static constructors are parameterless. If you don't provide a static constructor to initialize static fields, the C# compiler initializes static fields to their default value

```csharp
public class Person
{
  private string last;
  private string first;

  public Person(string lastName, string firstName)
  {
    last = lastName;
    first = firstName;
  }
}

public class Adult : Person
{
  public static int minimumAge;

  public Adult(string lastName, string firstName) : base(lastName, firstName)
  { }

  static Adult() => minimumAge = 18;
}

var adult = new Adult("Doe", "John");

Console.WriteLine($"Minimum age for adults: {Adult.minimumAge}");
```

#### Primary Constructor

```csharp
public class NamedItem(string name)
{
  public string Name => name;
}
```

### Finalizers

Finalizers (historically referred to as destructors) are used to perform any necessary final clean-up when a class instance is being collected by the garbage collector.

```csharp
class First
{
  ~First()
  {
    System.Diagnostics.Trace.WriteLine("First's finalizer is called.");
  }
}

class Second : First
{
  ~Second()
  {
    System.Diagnostics.Trace.WriteLine("Second's finalizer is called.");
  }
}

class Third : Second
{
  ~Third()
  {
    System.Diagnostics.Trace.WriteLine("Third's finalizer is called.");
  }
}

/* 
Test with code like the following:
  Third t = new Third();
  t = null;

When objects are finalized, the output would be:
Third's finalizer is called.
Second's finalizer is called.
First's finalizer is called.
*/
```

### Nested Types

A type defined within a class, struct, or interface is called a nested type. Regardless of whether the outer type is a class, interface, or struct, nested types default to private; they are accessible only from their containing type.

```csharp
public class Container
{
  class Nested
  {
    Nested() { }
  }
}
```

### Access Modifiers for Members

All types and type members have an accessibility level. The accessibility level controls whether they can be used from other code in your assembly or other assemblies. An assembly is a .dll or .exe created by compiling one or more .cs files in a single compilation.

- **public:** Code in any assembly can access this type or member. The accessibility level of the containing type controls the accessibility level of public members of the type.
- **private:** Only code declared in the same class or struct can access this member.
- **protected:** Only code in the same class or in a derived class can access this type or member.
- **internal:** Only code in the same assembly can access this type or member.
- **protected internal:** Only code in the same assembly or in a derived class in another assembly can access this type or member.
- **private protected:** Only code in the same assembly and in the same class or a derived class can access the type or member.
- **file:** Only code in the same file can access the type or member.

Struct members can't be declared as protected, protected internal, or private protected because structs don't support inheritance.

## Practical Examples and Exercises
```csharp
// Quick reference for common class patterns

// 1. Singleton Pattern
public sealed class Singleton
{
    private static readonly Lazy<Singleton> _instance = new(() => new Singleton());
    public static Singleton Instance => _instance.Value;
    private Singleton() { }
}

// 2. Builder Pattern
public class PersonBuilder
{
    private Person _person = new();
    public PersonBuilder WithName(string name) { _person.Name = name; return this; }
    public PersonBuilder WithAge(int age) { _person.Age = age; return this; }
    public Person Build() => _person;
}

// 3. Factory Pattern
public static class PersonFactory
{
    public static Person CreateStudent(string name) => new Person { Name = name, Type = "Student" };
    public static Person CreateTeacher(string name) => new Person { Name = name, Type = "Teacher" };
}

// 4. Observer Pattern using Events
public class Subject
{
    public event Action<string> StateChanged;
    private string _state;
    public string State 
    { 
        get => _state; 
        set { _state = value; StateChanged?.Invoke(value); } 
    }
}
```

## Additional Resources

### Official Documentation
- [Microsoft C# Classes Documentation](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/types/classes) - Comprehensive guide to C# classes
- [Inheritance in C#](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/inheritance) - Deep dive into inheritance concepts
- [Polymorphism](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/polymorphism) - Understanding polymorphism in C#
- [Access Modifiers](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/access-modifiers) - Complete guide to access control

### Object-Oriented Programming Concepts
- [Encapsulation](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/#encapsulation) - Data hiding and encapsulation principles
- [Abstraction](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/abstract-and-sealed-classes-and-class-members) - Abstract classes and members
- [Interfaces](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/types/interfaces) - Interface implementation and contracts

### Advanced Class Features
- [Extension Methods](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods) - Adding methods to existing types
- [Partial Classes](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) - Splitting class definitions across files
- [Generic Classes](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/generics/generic-classes) - Creating type-safe generic classes
- [Anonymous Types](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/types/anonymous-types) - Creating types on-the-fly

### Design Patterns and Best Practices
- [SOLID Principles](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles) - Object-oriented design principles
- [Design Patterns in C#](https://refactoring.guru/design-patterns/csharp) - Common design patterns implementation
- [Clean Code Practices](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions) - C# coding conventions and best practices

### Memory Management and Performance
- [Garbage Collection](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/) - Understanding .NET garbage collection
- [Finalizers and Dispose Pattern](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-dispose) - Proper resource cleanup
- [Value Types vs Reference Types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-types) - Understanding memory allocation

### Advanced Topics
- [Reflection](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/reflection) - Runtime type inspection and manipulation
- [Attributes](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/attributes/) - Metadata and attribute programming
- [Operator Overloading Guidelines](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/operator-overloads) - Best practices for operator overloading

### Learning Resources
- **Books:**
  - "C# in Depth" by Jon Skeet - Advanced C# concepts and internals
  - "Effective C#" by Bill Wagner - Best practices and idioms
  - "Clean Code" by Robert C. Martin - General programming principles
  - "Design Patterns: Elements of Reusable Object-Oriented Software" by Gang of Four

- **Online Courses:**
  - [Microsoft Learn C# Path](https://docs.microsoft.com/en-us/learn/paths/csharp-first-steps/) - Interactive C# learning
  - [Pluralsight C# Courses](https://www.pluralsight.com/browse/software-development/c-sharp) - Professional C# training
  - [Udemy C# Courses](https://www.udemy.com/topic/c-sharp/) - Various C# courses for all levels

### Tools and IDEs
- **Visual Studio** - Full-featured IDE with IntelliSense and debugging
- **Visual Studio Code** - Lightweight editor with C# extension
- **JetBrains Rider** - Cross-platform .NET IDE
- **LINQPad** - Interactive C# query tool
- **dotMemory/dotTrace** - Memory and performance profiling tools

### Code Analysis and Quality
- [Code Analysis Rules](https://docs.microsoft.com/en-us/dotnet/fundamentals/code-analysis/overview) - Static code analysis
- [EditorConfig](https://docs.microsoft.com/en-us/dotnet/fundamentals/code-analysis/code-style-rule-options) - Consistent coding styles
- [StyleCop](https://github.com/DotNetAnalyzers/StyleCopAnalyzers) - Code style analyzers
- [SonarQube](https://www.sonarqube.org/) - Code quality and security analysis

### Community and Forums
- [Stack Overflow C# Tag](https://stackoverflow.com/questions/tagged/c%23) - Q&A community
- [r/csharp](https://www.reddit.com/r/csharp/) - Reddit C# community
- [C# Discord](https://discord.gg/csharp) - Real-time chat and help
- [.NET Foundation](https://dotnetfoundation.org/) - Official .NET community