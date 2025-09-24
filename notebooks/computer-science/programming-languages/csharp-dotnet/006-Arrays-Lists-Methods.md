# Methods on Arrays and Lists

## Table of Content

1. [Sort](#sort)
2. [Searching](#searching)
3. [Reverse](#reverse)
4. [Clear](#clear)
5. [Resize](#resize)
6. [Join](#join)
7. [Split](#split)
8. [Advanced Array and List Operations](#advanced-array-and-list-operations)
9. [Specialized Collections](#specialized-collections)
10. [Error Handling and Best Practices](#error-handling-and-best-practices)
11. [Modern C# Features with Collections](#modern-c-features-with-collections)
12. [Parallel Operations](#parallel-operations)
13. [Testing Collections](#testing-collections)
14. [Performance Benchmarking](#performance-benchmarking)
15. [Additional Resources](#additional-resources)

## Sort

### Array

```csharp
string[] pallets = [ "B14", "A11", "B12", "A13" ];

Array.Sort(pallets);

foreach (var pallet in pallets)
{
  Console.WriteLine($"-- {pallet}");
}
```

### List

```csharp
List<string> names = ["Fernando", "Ana", "Felipe", "Maria", "Bill"];

names.Sort();

foreach (var name in names) 
{
  Console.WriteLine($"Hello {name.ToUpper()}!");
}
```

## Searching

### Array

```csharp
string[] names = ["Fernando", "Ana", "Felipe", "Maria", "Bill"];

Console.WriteLine($"I found Fernando at index {Array.IndexOf(names, "Fernando")}");
```

### List

```csharp
List<string> names = ["Fernando", "Ana", "Felipe", "Maria", "Bill"];

Console.WriteLine($"I found Fernando at index {names.IndexOf("Fernando")}");
```

## Reverse

### Array

```csharp
string[] pallets = [ "A11", "A13", "B12", "B14" ];

Array.Reverse(pallets);

foreach (var pallet in pallets)
{
  Console.WriteLine($"-- {pallet}");
}
```

### List

```csharp
List<string> pallets = [ "A11", "A13", "B12", "B14" ];

pallets.Reverse();

foreach (var pallet in pallets)
{
  Console.WriteLine($"-- {pallet}");
}
```

## Clear

### Array

Blank the selected positions on the array.

```csharp
string[] pallets = [ "B14", "A11", "B12", "A13" ];

Array.Clear(pallets, 0, 2);

foreach (var pallet in pallets)
{
  Console.WriteLine($"-- {pallet}"); // null, null, B12, A13
}
```

### List

Removes all the values in the List.

```csharp
List<string> pallets = [ "B14", "A11", "B12", "A13" ];

pallets.Clear();

Console.WriteLine(pallets.Count);
```

## Resize

### Array

```csharp
string[] pallets =  ["B14", "A11", "B12", "A13" ];
Console.WriteLine("");

Array.Resize(ref pallets, 6);

pallets[4] = "C01";
pallets[5] = "C02";

foreach (var pallet in pallets)
{
  Console.WriteLine($"-- {pallet}");
}
```

### List

There's no resize method in List.

## Join

### Array

```csharp
char[] letters = ['a', 'b', 'c'];
string result = String.Join("|", letters); // a|b|c
Console.WriteLine(result);
```

### List

```csharp
List<char> letters = ['a', 'b', 'c'];
string result = String.Join("|", letters); // a|b|c
Console.WriteLine(result);
```

## Split

### Array

```csharp
string result = "123|456|789";
string[] items = result.Split('|');

foreach (string item in items)
{
  Console.WriteLine(item); // 123, 456, 789
}
```

### List

```csharp
string result = "123|456|789";
List<string> items = result.Split('|').ToList();

foreach (string item in items)
{
  Console.WriteLine(item); // 123, 456, 789
}
```

## Advanced Array and List Operations
```csharp
// Array Advanced Operations
int[] numbers = [5, 2, 8, 1, 9, 3];

// Binary Search (requires sorted array)
Array.Sort(numbers);
int index = Array.BinarySearch(numbers, 8); // Returns index of 8

// Find methods
int firstEven = Array.Find(numbers, x => x % 2 == 0); // First even number
int[] allEvens = Array.FindAll(numbers, x => x % 2 == 0); // All even numbers
int lastOdd = Array.FindLast(numbers, x => x % 2 == 1); // Last odd number

// Exists and TrueForAll
bool hasEven = Array.Exists(numbers, x => x % 2 == 0); // True if any even
bool allPositive = Array.TrueForAll(numbers, x => x > 0); // True if all positive

// Copy operations
int[] copy = new int[numbers.Length];
Array.Copy(numbers, copy, numbers.Length);

// List Advanced Operations
List<int> numbersList = [5, 2, 8, 1, 9, 3];

// AddRange and InsertRange
numbersList.AddRange([10, 11, 12]);
numbersList.InsertRange(2, [100, 101]);

// RemoveAll with predicate
int removedCount = numbersList.RemoveAll(x => x > 10); // Remove all > 10

// Find methods (similar to Array)
int firstEvenInList = numbersList.Find(x => x % 2 == 0);
List<int> allEvensInList = numbersList.FindAll(x => x % 2 == 0);
int lastOddInList = numbersList.FindLast(x => x % 2 == 1);

// ConvertAll - transform to different type
List<string> stringNumbers = numbersList.ConvertAll(x => x.ToString());

// ForEach method
numbersList.ForEach(x => Console.WriteLine(x));

// GetRange - extract sub-list
List<int> subList = numbersList.GetRange(1, 3); // 3 elements starting at index 1

// Capacity management
numbersList.TrimExcess(); // Reduce capacity to actual count
numbersList.Capacity = 100; // Set specific capacity

// Binary search in sorted list
numbersList.Sort();
int listIndex = numbersList.BinarySearch(8);
```

## Specialized Collections
```csharp
// Dictionary for key-value pairs
Dictionary<string, int> ages = new()
{
    ["Alice"] = 30,
    ["Bob"] = 25,
    ["Charlie"] = 35
};

// HashSet for unique values
HashSet<string> uniqueNames = ["Alice", "Bob", "Alice"]; // Only Alice, Bob

// Queue (FIFO)
Queue<string> queue = new();
queue.Enqueue("First");
queue.Enqueue("Second");
string first = queue.Dequeue(); // "First"

// Stack (LIFO)
Stack<string> stack = new();
stack.Push("First");
stack.Push("Second");
string last = stack.Pop(); // "Second"

// SortedList for ordered key-value pairs
SortedList<int, string> sortedList = new()
{
    [3] = "Third",
    [1] = "First", 
    [2] = "Second"
}; // Automatically sorted by key

// LinkedList for efficient insertion/removal
LinkedList<int> linkedList = new([1, 2, 3, 4, 5]);
linkedList.AddFirst(0); // Add at beginning
linkedList.AddLast(6);  // Add at end
LinkedListNode<int> node = linkedList.Find(3);
linkedList.AddAfter(node, 10); // Add after specific node
```

## Error Handling and Best Practices
```csharp
// Safe array access
public static T SafeGetAt<T>(T[] array, int index, T defaultValue = default)
{
    if (array == null || index < 0 || index >= array.Length)
        return defaultValue;
    return array[index];
}

// Safe list operations
public static void SafeAddRange<T>(List<T> list, IEnumerable<T> items)
{
    if (list == null || items == null) return;
    list.AddRange(items);
}

// Null-safe enumeration
public static void PrintItems<T>(IEnumerable<T> items)
{
    if (items == null) return;
    
    foreach (var item in items)
    {
        Console.WriteLine(item?.ToString() ?? "null");
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

## Modern C# Features with Collections
```csharp
// Collection expressions (C# 12)
int[] array = [1, 2, 3, 4, 5];
List<string> list = ["apple", "banana", "cherry"];

// Pattern matching with collections
string AnalyzeArray(int[] numbers) => numbers switch
{
    [] => "Empty array",
    [var single] => $"Single element: {single}",
    [var first, var second] => $"Two elements: {first}, {second}",
    [var first, .., var last] => $"Multiple elements, first: {first}, last: {last}",
    _ => "Unknown pattern"
};

// Range and Index operators
int[] numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
int[] slice = numbers[2..8];        // Elements 2-7
int[] fromStart = numbers[..5];     // First 5 elements
int[] toEnd = numbers[5..];         // From index 5 to end
int lastElement = numbers[^1];      // Last element
int[] lastThree = numbers[^3..];    // Last 3 elements

// Record types with collections
public record Person(string Name, List<string> Hobbies);

// Init-only collections
public class ReadOnlyData
{
    public List<string> Items { get; init; } = new();
}

// Required collections
public class ConfigurationData
{
    public required List<string> RequiredSettings { get; init; }
}
```

## Parallel Operations
```csharp
// Parallel LINQ (PLINQ)
int[] largeArray = Enumerable.Range(1, 1000000).ToArray();

// Parallel processing
var evenNumbers = largeArray
    .AsParallel()
    .Where(x => x % 2 == 0)
    .ToArray();

// Parallel ForEach
Parallel.ForEach(largeArray, number =>
{
    // Process each number in parallel
    var result = ExpensiveOperation(number);
    Console.WriteLine(result);
});

// Concurrent collections for thread-safe operations
ConcurrentBag<int> concurrentBag = new();
ConcurrentQueue<int> concurrentQueue = new();
ConcurrentStack<int> concurrentStack = new();
ConcurrentDictionary<string, int> concurrentDict = new();
```

## Testing Collections
```csharp
// Unit testing with collections
[Test]
public void List_AddAndRemove_CountUpdatedCorrectly()
{
    var list = new List<string>();
    
    list.Add("item1");
    list.Add("item2");
    Assert.AreEqual(2, list.Count);
    
    list.Remove("item1");
    Assert.AreEqual(1, list.Count);
    Assert.Contains("item2", list);
}

// Testing with CollectionAssert
[Test]
public void Array_Sort_ProducesCorrectOrder()
{
    string[] input = ["c", "a", "b"];
    string[] expected = ["a", "b", "c"];
    
    Array.Sort(input);
    
    CollectionAssert.AreEqual(expected, input);
}

// Property-based testing
[Test]
public void List_Reverse_TwiceReturnsOriginal()
{
    var original = new List<int> { 1, 2, 3, 4, 5 };
    var copy = new List<int>(original);
    
    copy.Reverse();
    copy.Reverse();
    
    CollectionAssert.AreEqual(original, copy);
}
```

## Performance Benchmarking
```csharp
// Benchmarking with BenchmarkDotNet
[MemoryDiagnoser]
public class CollectionBenchmark
{
    private readonly int[] _array = Enumerable.Range(1, 1000).ToArray();
    private readonly List<int> _list = Enumerable.Range(1, 1000).ToList();

    [Benchmark]
    public int ArraySum()
    {
        int sum = 0;
        foreach (int item in _array)
            sum += item;
        return sum;
    }

    [Benchmark]
    public int ListSum()
    {
        int sum = 0;
        foreach (int item in _list)
            sum += item;
        return sum;
    }

    [Benchmark]
    public int LinqSum() => _array.Sum();

    [Benchmark]
    public int ParallelLinqSum() => _array.AsParallel().Sum();
}
```

## Additional Resources

### Official Documentation
- [Microsoft Arrays Documentation](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/) - Comprehensive guide to C# arrays
- [Array Class](https://docs.microsoft.com/en-us/dotnet/api/system.array) - Static methods for array manipulation
- [List<T> Class](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1) - Generic list implementation
- [IEnumerable<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.ienumerable-1) - Base enumeration interface
- [Collections Overview](https://docs.microsoft.com/en-us/dotnet/standard/collections/) - .NET collection types overview

### Array-Specific Resources
- [Multidimensional Arrays](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/multidimensional-arrays) - 2D and higher dimension arrays
- [Jagged Arrays](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/jagged-arrays) - Arrays of arrays
- [Array Covariance](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/arrays-as-objects) - Type relationships in arrays
- [Memory<T> and Span<T>](https://docs.microsoft.com/en-us/dotnet/standard/memory-and-spans/) - High-performance array operations

### List and Collection Types
- [Collection Interfaces](https://docs.microsoft.com/en-us/dotnet/standard/collections/commonly-used-collection-types) - IList, ICollection, IEnumerable
- [ObservableCollection<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.objectmodel.observablecollection-1) - Collections with change notifications
- [ReadOnlyCollection<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.objectmodel.readonlycollection-1) - Immutable wrapper collections
- [ConcurrentBag<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentbag-1) - Thread-safe collections

### Performance Considerations
- [When to Use Arrays vs Lists](https://docs.microsoft.com/en-us/dotnet/standard/collections/when-to-use-generic-collections) - Performance trade-offs
- [ArrayList vs List<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.arraylist) - Generic vs non-generic collections
- [Memory Allocation](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/) - Understanding collection memory usage
- [Big O Complexity](https://docs.microsoft.com/en-us/dotnet/standard/collections/selecting-a-collection-class#algorithmic-complexity) - Time complexity of operations

### LINQ with Arrays and Lists
- [LINQ to Objects](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/linq-to-objects) - Querying collections
- [Standard Query Operators](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/standard-query-operators-overview) - LINQ methods reference
- [Method Syntax vs Query Syntax](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) - Different LINQ approaches
- [Deferred Execution](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/deferred-execution-example) - Understanding lazy evaluation

### Learning Resources
- **Books:**
  - "C# in Depth" by Jon Skeet - Advanced collection concepts
  - "Effective C#" by Bill Wagner - Best practices for collections
  - "Pro .NET Performance" by Sasha Goldshtein - Performance optimization
  - "Concurrency in C# Cookbook" by Stephen Cleary - Thread-safe collections

- **Online Courses:**
  - [Microsoft Learn Collections](https://docs.microsoft.com/en-us/learn/modules/csharp-arrays/) - Interactive collection tutorials
  - [Pluralsight .NET Collections](https://www.pluralsight.com/courses/working-with-nulls-csharp) - Professional training
  - [Udemy Data Structures](https://www.udemy.com/topic/data-structures/) - Data structure fundamentals

### Community Resources
- [Stack Overflow Collections](https://stackoverflow.com/questions/tagged/collections+c%23) - Q&A community
- [C# Discord](https://discord.gg/csharp) - Real-time help and discussions
- [Reddit r/csharp](https://www.reddit.com/r/csharp/) - Community discussions
- [.NET Foundation](https://dotnetfoundation.org/) - Official .NET community
- [GitHub .NET Runtime](https://github.com/dotnet/runtime) - Source code for collections

### Tools and Extensions
- **Visual Studio Extensions:**
  - Collection Visualizer - View collection contents during debugging
  - Memory Usage Tool - Analyze collection memory consumption
  - Performance Profiler - Measure collection operation performance

- **NuGet Packages:**
  - [System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/) - Immutable collections
  - [MoreLINQ](https://www.nuget.org/packages/morelinq/) - Additional LINQ operators
  - [BenchmarkDotNet](https://www.nuget.org/packages/BenchmarkDotNet/) - Performance benchmarking

### Real-World Use Cases
- **Data Processing:** Sorting large datasets, filtering records, aggregating results
- **Caching:** Using dictionaries for lookup tables, implementing LRU caches
- **Configuration:** Managing application settings with collections
- **APIs:** Handling request/response collections, pagination
- **Game Development:** Managing entities, inventories, game states
- **Financial Systems:** Processing transactions, calculating totals, managing portfolios
```