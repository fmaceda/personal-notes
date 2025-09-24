# Arrays, Lists, and Collections

## Table of Content

1. [Arrays](#arrays)
   - [Adding Elements](#adding-elements)
   - [Ranges in Arrays](#ranges-in-arrays)
2. [Lists](#lists)
   - [Adding Elements](#adding-elements-1)
   - [Removing Elements](#removing-elements)
   - [Ranges in Lists](#ranges-in-lists)
3. [Modern C# Collection Features](#modern-c-collection-features)
4. [Specialized Collections](#specialized-collections)
5. [Advanced Operations and Algorithms](#advanced-operations-and-algorithms)
6. [Error Handling and Best Practices](#error-handling-and-best-practices)
7. [Performance Tips and Benchmarking](#performance-tips-and-benchmarking)
8. [Additional Resources](#additional-resources)

## Arrays

Static list of elements. The size is defined at the beginning and cannot be changed.

```csharp
string[] names = {"Fernando", "Ana", "Felipe"};

for (int i = 0; i < names.Length; i++)
{
  // Prints three items in the list
  Console.WriteLine($"Hello {names[i].ToUpper()}!");
}

foreach (var name in names)
{
  // Prints three items in the list
  Console.WriteLine($"Hello {name.ToUpper()}!");
}
```

### Adding Elements

```csharp
string[] names = ["Fernando", "Ana", "Felipe"];

// Adding new elements.
names = [..names, "Maria"];

foreach (var name in names)
{
  // Prints four items in the list, adding the new two ones and removing one of them.
  Console.WriteLine($"Hello {name.ToUpper()}!");
} 
```

### Ranges in Arrays

```csharp
string[] names = ["Fernando", "Ana", "Felipe", "Maria", "Bill"];

// Prints names from the index 2 to index 3 (the right side of the range is not inclusive)
foreach (var name in names[2..4]) 
{
  // Prints four items in the list, adding the new two ones and removing one of them.
  Console.WriteLine($"Hello {name.ToUpper()}!");
}

// Get's the first one in the list.
Console.WriteLine($"Hello {names[0].ToUpper()}!");

// Get's the first one in the list from the back.
Console.WriteLine($"Hello {names[^1].ToUpper()}!");
```

## Lists

Dynamic list of elements. The size can be changed by adding or removing elements.

```csharp
using System;
using System.Collections.Generic;

var names = new List<string> {"Fernando", "Ana", "Felipe"};

for (int i = 0; i < names.Count; i++)
{
  // Prints three items in the list
  Console.WriteLine($"Hello {names[i].ToUpper()}!");
}

foreach (var name in names)
{
  // Prints three items in the list
  Console.WriteLine($"Hello {name.ToUpper()}!");
}
```

### Adding Elements

```csharp
List<string> names = ["Fernando", "Ana", "Felipe"];

names.Add("Maria");
names.Add("Bill");

foreach (var name in names)
{
  Console.WriteLine($"Hello {name.ToUpper()}!");
}
```

### Removing Elements

```csharp
List<string> names = ["Fernando", "Ana", "Felipe"];

names.Remove("Ana");

foreach (var name in names)
{
  Console.WriteLine($"Hello {name.ToUpper()}!");
}
```

### Ranges in Lists

```csharp
List<string> names = ["Fernando", "Ana", "Felipe", "Maria", "Bill"];

// Prints names from the index 2 to index 3 (the right side of the range is not inclusive)
foreach (var name in names[2..4]) 
{
  // Prints four items in the list, adding the new two ones and removing one of them.
  Console.WriteLine($"Hello {name.ToUpper()}!");
}

// Get's the first one in the list.
Console.WriteLine($"Hello {names[0].ToUpper()}!");

// Get's the first one in the list from the back.
Console.WriteLine($"Hello {names[^1].ToUpper()}!");
```

## Modern C# Collection Features
```csharp
// Collection expressions (C# 12)
int[] array = [1, 2, 3, 4, 5];
List<string> list = ["apple", "banana", "cherry"];
Dictionary<string, int> dict = ["key1" = 1, "key2" = 2];

// Pattern matching with collections
string AnalyzeCollection<T>(IEnumerable<T> items) => items switch
{
    [] => "Empty collection",
    [var single] => $"Single item: {single}",
    [var first, var second] => $"Two items: {first}, {second}",
    [var first, .., var last] => $"Multiple items, first: {first}, last: {last}",
    _ => "Unknown pattern"
};

// Range and Index operators
int[] numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
int[] slice = numbers[2..8];        // Elements 2-7
int[] fromStart = numbers[..5];     // First 5 elements
int[] toEnd = numbers[5..];         // From index 5 to end
int lastElement = numbers[^1];      // Last element
int[] lastThree = numbers[^3..];    // Last 3 elements

// Spread operator for arrays
int[] first = [1, 2, 3];
int[] second = [4, 5, 6];
int[] combined = [..first, ..second]; // [1, 2, 3, 4, 5, 6]
int[] withExtra = [0, ..first, 10, ..second, 11]; // [0, 1, 2, 3, 10, 4, 5, 6, 11]
```

## Specialized Collections
```csharp
// Immutable Collections (System.Collections.Immutable)
using System.Collections.Immutable;

ImmutableArray<int> immutableArray = [1, 2, 3, 4, 5];
ImmutableList<string> immutableList = ["a", "b", "c"];
ImmutableDictionary<string, int> immutableDict = new Dictionary<string, int>
{
    ["key1"] = 1,
    ["key2"] = 2
}.ToImmutableDictionary();

// Observable Collections (WPF/UI scenarios)
using System.Collections.ObjectModel;
ObservableCollection<string> observableList = new() { "item1", "item2" };
observableList.CollectionChanged += (s, e) => Console.WriteLine("Collection changed!");

// Concurrent Collections (Thread-safe)
using System.Collections.Concurrent;
ConcurrentBag<int> concurrentBag = new();
ConcurrentQueue<string> concurrentQueue = new();
ConcurrentStack<double> concurrentStack = new();
ConcurrentDictionary<string, object> concurrentDict = new();

// ReadOnly Collections
ReadOnlyCollection<int> readOnlyList = new List<int> { 1, 2, 3 }.AsReadOnly();
IReadOnlyList<string> readOnlyInterface = new List<string> { "a", "b", "c" };
```

## Advanced Operations and Algorithms
```csharp
// Custom sorting with IComparer
public class StringLengthComparer : IComparer<string>
{
    public int Compare(string x, string y) => x?.Length.CompareTo(y?.Length) ?? 0;
}

string[] words = ["apple", "cat", "elephant", "dog"];
Array.Sort(words, new StringLengthComparer());

// Binary search (requires sorted array)
int[] sortedNumbers = [1, 3, 5, 7, 9, 11, 13, 15];
int index = Array.BinarySearch(sortedNumbers, 7); // Returns index of 7

// List with custom capacity for performance
List<int> efficientList = new(capacity: 1000); // Pre-allocate space

// Converting between collection types
int[] array = [1, 2, 3, 4, 5];
List<int> listFromArray = array.ToList();
HashSet<int> setFromArray = array.ToHashSet();
Dictionary<int, string> dictFromArray = array.ToDictionary(x => x, x => x.ToString());

// Filtering and transformation
var evenNumbers = array.Where(x => x % 2 == 0).ToArray();
var squaredNumbers = array.Select(x => x * x).ToList();
var filteredAndTransformed = array
    .Where(x => x > 2)
    .Select(x => x.ToString())
    .ToHashSet();
```

## Error Handling and Best Practices
```csharp
// Safe array/list access
public static T SafeGetAt<T>(IList<T> list, int index, T defaultValue = default)
{
    return index >= 0 && index < list.Count ? list[index] : defaultValue;
}

// Null-safe enumeration
public static void SafeForEach<T>(IEnumerable<T> items, Action<T> action)
{
    if (items == null || action == null) return;
    
    foreach (var item in items)
    {
        action(item);
    }
}

// ArgumentNull checking
public static void ProcessItems<T>(IEnumerable<T> items)
{
    ArgumentNullException.ThrowIfNull(items); // C# 11+
    
    foreach (var item in items)
    {
        // Process item
    }
}

// Defensive copying
public static T[] CreateSafeCopy<T>(T[] original)
{
    if (original == null) return null;
    
    var copy = new T[original.Length];
    Array.Copy(original, copy, original.Length);
    return copy;
}
```

## Performance Tips and Benchmarking
```csharp
// Using BenchmarkDotNet for performance testing
[MemoryDiagnoser]
public class CollectionPerformanceBenchmark
{
    private readonly int[] _sourceArray = Enumerable.Range(1, 10000).ToArray();

    [Benchmark]
    public int[] ArrayCopy() => (int[])_sourceArray.Clone();

    [Benchmark]
    public int[] ArrayToArray() => _sourceArray.ToArray();

    [Benchmark]
    public List<int> ArrayToList() => _sourceArray.ToList();

    [Benchmark]
    public int SumWithLoop()
    {
        int sum = 0;
        foreach (int item in _sourceArray)
            sum += item;
        return sum;
    }

    [Benchmark]
    public int SumWithLinq() => _sourceArray.Sum();

    [Benchmark]
    public int SumWithSpan()
    {
        ReadOnlySpan<int> span = _sourceArray.AsSpan();
        int sum = 0;
        foreach (int item in span)
            sum += item;
        return sum;
    }
}
```

## Additional Resources

### Official Documentation
- [Microsoft Arrays Documentation](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/) - Comprehensive guide to C# arrays
- [Array Class](https://docs.microsoft.com/en-us/dotnet/api/system.array) - Static methods and properties for arrays
- [List<T> Class](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1) - Generic list implementation
- [Collections Overview](https://docs.microsoft.com/en-us/dotnet/standard/collections/) - Complete .NET collections overview
- [IEnumerable<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) - Base enumeration interface

### Collection Types and Interfaces
- [IList<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ilist-1) - Generic list interface
- [ICollection<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.icollection-1) - Generic collection interface
- [Dictionary<TKey,TValue>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2) - Key-value pair collections
- [HashSet<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.hashset-1) - Unique element collections
- [Queue<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.queue-1) - First-in, first-out collections
- [Stack<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.stack-1) - Last-in, first-out collections

### Advanced Array and List Features
- [Multidimensional Arrays](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/multidimensional-arrays) - 2D and higher dimension arrays
- [Jagged Arrays](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/jagged-arrays) - Arrays of arrays
- [Array Covariance](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/arrays-as-objects) - Type relationships in arrays
- [Indices and Ranges](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/index-range) - Modern indexing with ^ and .. operators
- [Collection Expressions](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/collection-expressions) - C# 12 collection literal syntax

### Memory and Performance
- [Memory<T> and Span<T>](https://docs.microsoft.com/en-us/dotnet/standard/memory-and-spans/) - High-performance memory operations
- [ArrayPool<T>](https://docs.microsoft.com/en-us/dotnet/api/system.buffers.arraypool-1) - Array pooling for performance
- [Performance Considerations](https://docs.microsoft.com/en-us/dotnet/standard/collections/when-to-use-generic-collections) - Choosing the right collection type
- [Big O Complexity](https://docs.microsoft.com/en-us/dotnet/standard/collections/selecting-a-collection-class#algorithmic-complexity) - Time complexity of operations

### LINQ and Functional Programming
- [LINQ to Objects](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/linq-to-objects) - Querying collections with LINQ
- [Standard Query Operators](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/standard-query-operators-overview) - LINQ methods reference
- [Enumerable Class](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable) - Extension methods for collections
- [Deferred Execution](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/deferred-execution-example) - Understanding lazy evaluation

### Learning Resources
- **Books:**
  - "C# in Depth" by Jon Skeet - Advanced collection concepts and LINQ
  - "Effective C#" by Bill Wagner - Best practices for collections
  - "Pro .NET Performance" by Sasha Goldshtein - Performance optimization
  - "Concurrency in C# Cookbook" by Stephen Cleary - Thread-safe collections

- **Online Courses:**
  - [Microsoft Learn Collections](https://docs.microsoft.com/en-us/learn/modules/csharp-arrays/) - Interactive tutorials
  - [Pluralsight .NET Collections](https://www.pluralsight.com/courses/working-with-nulls-csharp) - Professional training
  - [Udemy Data Structures and Algorithms](https://www.udemy.com/topic/data-structures/) - Fundamentals

- **Practice Platforms:**
  - [LeetCode](https://leetcode.com/) - Algorithm and data structure problems
  - [HackerRank](https://www.hackerrank.com/) - Programming challenges
  - [Codewars](https://www.codewars.com/) - Code kata for collections

### Real-World Applications
- **Data Processing:** ETL operations, filtering large datasets, aggregations
- **Caching:** Implementing LRU caches with dictionaries and linked lists
- **Configuration Management:** Storing application settings in collections
- **Game Development:** Managing entities, inventories, leaderboards
- **Web APIs:** Handling request/response collections, pagination
- **Financial Systems:** Processing transactions, portfolio management
- **Machine Learning:** Feature vectors, dataset manipulation

### Community Resources
- [Stack Overflow Collections](https://stackoverflow.com/questions/tagged/collections+c%23) - Q&A community
- [C# Discord](https://discord.gg/csharp) - Real-time help and discussions
- [Reddit r/csharp](https://www.reddit.com/r/csharp/) - Community discussions
- [.NET Foundation](https://dotnetfoundation.org/) - Official .NET community
- [GitHub Awesome .NET](https://github.com/quozd/awesome-dotnet) - Curated .NET resources

### Tools and Libraries
- **NuGet Packages:**
  - [System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/) - Immutable collections
  - [MoreLINQ](https://www.nuget.org/packages/morelinq/) - Additional LINQ operators
  - [BenchmarkDotNet](https://www.nuget.org/packages/BenchmarkDotNet/) - Performance benchmarking
  - [FluentAssertions](https://www.nuget.org/packages/FluentAssertions/) - Better test assertions

- **Development Tools:**
  - Visual Studio Collection Debugger Visualizers
  - Memory usage profilers (dotMemory, PerfView)
  - Code analysis tools (SonarQube, CodeMaid)
  - Performance profilers (dotTrace, Application Insights)