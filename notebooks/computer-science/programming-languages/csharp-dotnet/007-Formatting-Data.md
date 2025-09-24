# Formatting Data

## Table of Content

1. [Composite Formatting](#composite-formatting)
2. [Formatting Currency](#formatting-currency)
3. [Formatting Numbers](#formatting-numbers)
4. [Formatting Percentage](#formatting-percentage)
5. [Padding](#padding)
6. [Advanced Formatting Techniques](#advanced-formatting-techniques)
7. [Common Formatting Patterns](#common-formatting-patterns)
8. [Performance Benchmarking](#performance-benchmarking)
9. [Additional Resources](#additional-resources)

## Composite Formatting

```csharp
string first = "Hello";
string second = "World";
string result = string.Format("{0} {1}!", first, second);
Console.WriteLine(result);
```

## Formatting Currency

```csharp
decimal price = 123.45m;
int discount = 50;
Console.WriteLine($"Price: {price:C} (Save {discount:C})");
```

## Formatting Numbers

```csharp
decimal measurement = 123456.78912m;
Console.WriteLine($"Measurement: {measurement:N} units");
Console.WriteLine($"Measurement: {measurement:N4} units");
```

## Formatting Percentage

```csharp
decimal tax = .36785m;
Console.WriteLine($"Tax rate: {tax:P2}");
```

## Padding

```csharp
string input = "Pad this";
Console.WriteLine(input.PadLeft(12)); // " 	Pad this"
Console.WriteLine(input.PadLeft(12, '*')); // "****Pad this"
Console.WriteLine(input.PadRight(12, '*')); // "Pad this****"
```

## Advanced Formatting Techniques
```csharp
// Custom Format Providers
public class CustomNumberFormatInfo : IFormatProvider, ICustomFormatter
{
    public object GetFormat(Type formatType) =>
        formatType == typeof(ICustomFormatter) ? this : null;

    public string Format(string format, object arg, IFormatProvider formatProvider)
    {
        if (arg is decimal value && format == "MONEY")
            return $"${value:N2} USD";
        
        return arg?.ToString() ?? string.Empty;
    }
}

decimal amount = 1234.56m;
string formatted = string.Format(new CustomNumberFormatInfo(), "{0:MONEY}", amount);
// Output: $1,234.56 USD

// Alignment and Padding in Interpolation
string name = "Alice";
int age = 30;
Console.WriteLine($"|{name,-10}|{age,5}|"); // |Alice     |   30|

// Format Specifiers in Interpolation
DateTime now = DateTime.Now;
Console.WriteLine($"Date: {now:yyyy-MM-dd}, Time: {now:HH:mm:ss}");

// Conditional Formatting
int score = 85;
string grade = score switch
{
    >= 90 => "A",
    >= 80 => "B", 
    >= 70 => "C",
    >= 60 => "D",
    _ => "F"
};
Console.WriteLine($"Score: {score:N0} ({grade})");
```

## Common Formatting Patterns
```csharp
// File Sizes
long bytes = 1536;
string[] sizes = { "B", "KB", "MB", "GB", "TB" };
double len = bytes;
int order = 0;
while (len >= 1024 && order < sizes.Length - 1)
{
    order++;
    len = len / 1024;
}
string fileSize = $"{len:0.##} {sizes[order]}"; // "1.5 KB"

// Phone Numbers
string phoneNumber = "1234567890";
string formatted = $"({phoneNumber[..3]}) {phoneNumber[3..6]}-{phoneNumber[6..]}";
// Output: (123) 456-7890

// Credit Card Masking
string creditCard = "1234567890123456";
string masked = $"****-****-****-{creditCard[^4..]}"; // ****-****-****-3456

// Progress Bars
double progress = 0.75;
int barLength = 20;
int filled = (int)(progress * barLength);
string progressBar = $"[{new string('█', filled)}{new string('░', barLength - filled)}] {progress:P0}";
// Output: [███████████████░░░░░] 75%

// Table Formatting
var data = new[]
{
    new { Name = "Alice", Age = 30, Salary = 75000 },
    new { Name = "Bob", Age = 25, Salary = 65000 },
    new { Name = "Charlie", Age = 35, Salary = 85000 }
};

Console.WriteLine($"{"Name",-10} {"Age",3} {"Salary",10}");
Console.WriteLine(new string('-', 25));
foreach (var person in data)
{
    Console.WriteLine($"{person.Name,-10} {person.Age,3} {person.Salary,10:C0}");
}
```

## Performance Benchmarking
```csharp
// Example using BenchmarkDotNet
[MemoryDiagnoser]
public class StringFormattingBenchmark
{
    private const string Name = "Alice";
    private const int Age = 30;

    [Benchmark]
    public string StringConcatenation() => "Name: " + Name + ", Age: " + Age;

    [Benchmark]
    public string StringFormat() => string.Format("Name: {0}, Age: {1}", Name, Age);

    [Benchmark] 
    public string StringInterpolation() => $"Name: {Name}, Age: {Age}";

    [Benchmark]
    public string StringBuilder()
    {
        var sb = new System.Text.StringBuilder();
        sb.Append("Name: ").Append(Name).Append(", Age: ").Append(Age);
        return sb.ToString();
    }
}
```

## Additional Resources

### Official Documentation
- [Microsoft String Formatting Documentation](https://docs.microsoft.com/en-us/dotnet/standard/base-types/formatting-types) - Comprehensive guide to .NET formatting
- [Composite Formatting](https://docs.microsoft.com/en-us/dotnet/standard/base-types/composite-formatting) - String.Format and composite formatting
- [Standard Numeric Format Strings](https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-numeric-format-strings) - Built-in numeric format specifiers
- [Custom Numeric Format Strings](https://docs.microsoft.com/en-us/dotnet/standard/base-types/custom-numeric-format-strings) - Creating custom number formats
- [DateTime Format Strings](https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-date-and-time-format-strings) - Date and time formatting

### String Interpolation and Modern Formatting
- [String Interpolation](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/tokens/interpolated) - Modern C# string formatting syntax
- [FormattableString](https://docs.microsoft.com/en-us/dotnet/api/system.formattablestring) - Culture-aware string interpolation
- [DefaultInterpolatedStringHandler](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.compilerservices.defaultinterpolatedstringhandler) - C# 10.0+ performance improvements

### DateTime Formatting
- [Standard DateTime Format Strings](https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-date-and-time-format-strings) - Built-in date formats
- [Custom DateTime Format Strings](https://docs.microsoft.com/en-us/dotnet/standard/base-types/custom-date-and-time-format-strings) - Custom date patterns
- [TimeSpan Format Strings](https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-timespan-format-strings) - Time interval formatting
- [DateOnly and TimeOnly](https://docs.microsoft.com/en-us/dotnet/api/system.dateonly) - .NET 6+ date and time types

### Globalization and Localization
- [CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo) - Culture-specific formatting
- [NumberFormatInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.numberformatinfo) - Number formatting properties
- [DateTimeFormatInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.datetimeformatinfo) - Date/time formatting properties
- [Invariant Culture](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.invariantculture) - Culture-neutral formatting

### Performance Considerations
- [StringBuilder](https://docs.microsoft.com/en-us/dotnet/api/system.text.stringbuilder) - Efficient string building
- [Span<char> and ReadOnlySpan<char>](https://docs.microsoft.com/en-us/dotnet/standard/memory-and-spans/) - High-performance string operations
- [StringValues](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.primitives.stringvalues) - ASP.NET Core string optimization

### Specialized Formatting Libraries
- [Humanizer](https://github.com/Humanizr/Humanizer) - Human-friendly formatting for dates, numbers, etc.
- [NodaTime](https://nodatime.org/) - Better date and time API for .NET
- [System.Text.Json](https://docs.microsoft.com/en-us/dotnet/standard/serialization/system-text-json-overview) - JSON formatting and serialization
- [CsvHelper](https://joshclose.github.io/CsvHelper/) - CSV formatting and parsing

### Error Handling in Formatting
- [FormatException](https://docs.microsoft.com/en-us/dotnet/api/system.formatexception) - Invalid format strings
- [ArgumentNullException](https://docs.microsoft.com/en-us/dotnet/api/system.argumentnullexception) - Null format arguments
- [TryParse Methods](https://docs.microsoft.com/en-us/dotnet/standard/base-types/parsing-strings) - Safe parsing with formatting
- [Format Validation](https://docs.microsoft.com/en-us/dotnet/standard/base-types/formatting-types#format-strings-and-net-types) - Ensuring format compatibility

### Learning Resources
- **Books:**
  - ".NET Core in Action" by Dustin Metzgar - Comprehensive .NET formatting
  - "C# in Depth" by Jon Skeet - Advanced string and formatting concepts
  - "Pro .NET Performance" by Sasha Goldshtein - Performance optimization including strings
  - "Programming C# 10.0" by Ian Griffiths - Modern C# formatting features

- **Online Courses:**
  - [Microsoft Learn String Formatting](https://docs.microsoft.com/en-us/learn/modules/csharp-format-strings/) - Interactive formatting tutorials

### Real-World Applications
- **Financial Applications:** Currency formatting, decimal precision, accounting formats
- **Reporting Systems:** Table formatting, data alignment, summary reports
- **User Interfaces:** Display formatting, input validation, localization
- **APIs and Web Services:** JSON formatting, response formatting, error messages
- **Logging Systems:** Structured logging, timestamp formatting, message formatting

### Tools and Extensions
- **Visual Studio Extensions:**
  - String Resource Visualizer - Preview string resources
  - Format Document - Code formatting tools
  - Regex Tester - Pattern matching for formats

- **Online Tools:**
  - [Regex101](https://regex101.com/) - Test formatting patterns

### Community Resources
- [Stack Overflow String Formatting](https://stackoverflow.com/questions/tagged/string-formatting+c%23) - Q&A community
- [C# Discord](https://discord.gg/csharp) - Real-time help and discussions
- [Reddit r/csharp](https://www.reddit.com/r/csharp/) - Community discussions
- [.NET Foundation](https://dotnetfoundation.org/) - Official .NET community
- [C# Corner Formatting](https://www.c-sharpcorner.com/topics/string-formatting) - Articles and tutorials
