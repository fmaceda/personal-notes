# Events and Delegates

## Table of Content

1. [Delegates](#delegates)
   - [Strongly Typed Delegates](#strongly-typed-delegates)
2. [Events](#events)
3. [Common Delegate Use Cases](#common-delegate-use-cases)
3. [Additional Resources](#additional-resources)

## Delegates

Delegates provide a late binding mechanism in .NET. Late Binding means that you create an algorithm where the caller also supplies at least one method that implements part of the algorithm.

```csharp
// From the .NET Core library

// Define the delegate type:
public delegate int? Comparison<in T>(T left, T right);

// Declare an instance of that type:
public Comparison<string> comparer;

// Assign a method to the delegate:
private static int? CompareLength(string left, string right) =>
  left.Length.CompareTo(right.Length);
  
comparer += CompareLength; // This line will add the method to the delegate.

// comparer -= CompareLength; // This line will remove the method from the delegate.

// Invoke delegate.
int? result = comparer?.Invoke("Long String", "String");
// int result = comparer("Long String", "String"); // Other way to invoke the delegate.

Console.WriteLine(result); // Outputs: 1 (because "Long String" is longer than "String")
```

### Strongly Typed Delegates

The .NET Core framework contains several types that you can reuse whenever you need delegate types. These are generic definitions so you can declare customizations when you need new method declarations.

```csharp
// Action
public delegate void Action();
public delegate void Action<in T>(T arg);
public delegate void Action<in T1, in T2>(T1 arg1, T2 arg2);

// Func
public delegate TResult Func<out TResult>();
public delegate TResult Func<in T1, out TResult>(T1 arg);
public delegate TResult Func<in T1, in T2, out TResult>(T1 arg1, T2 arg2);

// Predicate
public delegate bool Predicate<in T>(T obj);
```

Action and Func delegates support until 16 in parameters.

## Events

Events are, like delegates, a late binding mechanism. In fact, events are built on the language support for delegates.

Events are a way for an object to broadcast (to all interested components in the system) that something happened. Any other component can subscribe to the event, and be notified when an event is raised.

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

## Common Delegate Use Cases
```csharp
// 1. Callback Functions
public void ProcessDataAsync(Action<string> onComplete)
{
    // Simulate async work
    Task.Run(() => {
        Thread.Sleep(1000);
        onComplete("Processing completed!");
    });
}

// 2. Event Handling
public class Timer
{
    public event Action<DateTime> Tick;
    
    public void Start()
    {
        // Simulate timer tick
        Tick?.Invoke(DateTime.Now);
    }
}

// 3. Functional Programming
public static class Extensions
{
    public static IEnumerable<TResult> Map<T, TResult>(
        this IEnumerable<T> source, 
        Func<T, TResult> selector) => source.Select(selector);
}

// 4. Configuration and Setup
public class ServiceBuilder
{
    public ServiceBuilder Configure(Action<ServiceOptions> configureOptions)
    {
        var options = new ServiceOptions();
        configureOptions(options);
        return this;
    }
}

// 5. Error Handling
public void ExecuteWithRetry(Action operation, Action<Exception> onError)
{
    try
    {
        operation();
    }
    catch (Exception ex)
    {
        onError(ex);
    }
}
```

## Additional Resources

### Official Documentation
- [Microsoft Delegates Documentation](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/) - Comprehensive guide to delegates in C#
- [Events](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/events/) - Understanding events and event handling
- [EventHandler](https://docs.microsoft.com/en-us/dotnet/api/system.eventhandler) - Built-in event delegate types
- [Action and Func Delegates](https://docs.microsoft.com/en-us/dotnet/api/system.action) - Generic delegate types

### Core Concepts
- [Multicast Delegates](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/using-delegates) - Understanding delegate invocation lists
- [Anonymous Methods](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/anonymous-methods) - Creating delegates without named methods
- [Lambda Expressions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) - Modern syntax for delegates
- [Closures](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/local-functions) - Variable capture in delegates

### Event Patterns and Best Practices
- [Observer Pattern](https://refactoring.guru/design-patterns/observer/csharp/example) - Classical implementation using events
- [Publisher-Subscriber Pattern](https://docs.microsoft.com/en-us/dotnet/standard/events/observer-design-pattern) - Event-driven architecture
- [Custom Event Arguments](https://docs.microsoft.com/en-us/dotnet/standard/events/how-to-raise-and-consume-events) - Creating meaningful event data
- [Weak Event Pattern](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/advanced/weak-event-patterns) - Preventing memory leaks

### Advanced Topics
- [Covariance and Contravariance](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/covariance-contravariance/) - Type variance in delegates
- [Asynchronous Events](https://docs.microsoft.com/en-us/dotnet/csharp/async) - Handling async event handlers
- [Thread Safety](https://docs.microsoft.com/en-us/dotnet/standard/threading/managed-threading-best-practices) - Thread-safe event handling
- [Performance Considerations](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/performance) - Optimizing delegate performance

### Functional Programming
- [Method Chaining](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods) - Fluent interfaces with delegates
- [Partial Application](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) - Currying and partial functions
- [LINQ and Delegates](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/getting-started-with-linq) - Functional queries

### Modern C# Features
- [Local Functions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/local-functions) - Alternative to simple delegates
- [Expression Trees](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/expression-trees/) - Compile-time delegate analysis
- [Pattern Matching](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/functional/pattern-matching) - Modern conditional logic
- [Switch Expressions](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/switch-expression) - Functional-style conditionals

### Learning Resources
- **Books:**
  - "C# in Depth" by Jon Skeet - Advanced delegate and event concepts
  - "Functional Programming in C#" by Enrico Buonanno - Functional programming with delegates
  - "Concurrency in C# Cookbook" by Stephen Cleary - Async and parallel programming
  - "CLR via C#" by Jeffrey Richter - Deep understanding of .NET runtime

- **Online Courses:**
  - [Pluralsight Functional Programming](https://www.pluralsight.com/courses/functional-programming-csharp) - Functional concepts in C#

### Practical Applications
- **UI Programming:**
  - WPF/WinUI event handling
  - Button clicks, property changes
  - Data binding and notifications

- **Asynchronous Programming:**
  - Callback patterns
  - Event-based async operations
  - Progress reporting

- **Design Patterns:**
  - Command pattern implementation
  - Strategy pattern with delegates
  - Template method pattern

### Testing and Debugging
- [Mocking Events](https://docs.microsoft.com/en-us/dotnet/core/testing/) - Unit testing event-driven code
- [Debugging Delegates](https://docs.microsoft.com/en-us/visualstudio/debugger/) - Visual Studio debugging techniques
- [Event Tracing](https://docs.microsoft.com/en-us/dotnet/core/diagnostics/) - Monitoring event flow
- [Performance Profiling](https://docs.microsoft.com/en-us/visualstudio/profiling/) - Analyzing delegate performance

### Framework Integration
- **ASP.NET Core:**
  - Middleware delegates
  - Request/response pipeline events
  - Dependency injection with delegates

- **Entity Framework:**
  - Change tracking events
  - Database operation callbacks
  - Custom conventions with delegates

- **SignalR:**
  - Real-time event broadcasting
  - Client-server communication
  - Hub method delegates

### Community Resources
- [Stack Overflow Delegates Tag](https://stackoverflow.com/questions/tagged/delegates) - Q&A community
- [C# Discord Events Channel](https://discord.gg/csharp) - Real-time discussions
- [Reddit r/csharp](https://www.reddit.com/r/csharp/) - Community discussions
- [.NET Foundation](https://dotnetfoundation.org/) - Official .NET community
- [C# Corner Events](https://www.c-sharpcorner.com/topics/events) - Articles and tutorials

### Tools and Libraries
- **Event Sourcing:**
  - EventStore - Event-driven data storage
  - MediatR - Mediator pattern implementation
  - Rebus - Service bus for .NET

- **Reactive Programming:**
  - Reactive Extensions (Rx.NET) - Reactive programming library
  - System.Reactive - LINQ to Events
  - DynamicData - Reactive collections

- **Testing:**
  - Moq - Mocking framework with delegate support
  - NSubstitute - Friendly mocking library
  - FluentAssertions - Fluent testing assertions
```