# Convert Data Types

## Table of Contents

1. [Type Conversion Methods](#type-conversion-methods)
   - [Casting](#casting)
   - [To String](#to-string)
   - [Parse](#parse)
   - [TryParse](#tryparse)
   - [Convert](#convert)
2. [Advanced Type Conversion Techniques](#advanced-type-conversion-techniques)
3. [Error Handling in Conversions](#error-handling-in-conversions)
4. [Culture-Aware Conversions](#culture-aware-conversions)
5. [Performance Considerations](#performance-considerations)
6. [JSON and XML Conversions](#json-and-xml-conversions)
7. [Database Type Conversions](#database-type-conversions)
8. [Additional Resources](#additional-resources)

## Type Conversion Methods

### Casting

Casting truncates the value.

```csharp
decimal myDecimal = 3.14m;
Console.WriteLine($"decimal: {myDecimal}");

int myInt = (int)myDecimal;
Console.WriteLine($"int: {myInt}");
```

### To String

```csharp
int first = 5;
int second = 7;
string message = first.ToString() + second.ToString();
Console.WriteLine(message);
```

### Parse

```csharp
string first = "5";
string second = "7";
int sum = int.Parse(first) + int.Parse(second);
Console.WriteLine(sum);
```

### TryParse

```csharp
string value = "102";
int result = 0;
if (int.TryParse(value, out result))
{
  Console.WriteLine($"Measurement: {result}");
}
else
{
  Console.WriteLine("Unable to report the measurement.");
}
```

### Convert

```csharp
string value1 = "5";
string value2 = "7";
int result = Convert.ToInt32(value1) * Convert.ToInt32(value2);
Console.WriteLine(result);
```

## Advanced Type Conversion Techniques
```csharp
// Safe casting with 'as' operator
object obj = "Hello";
string str = obj as string; // Returns null if cast fails
if (str != null)
{
    Console.WriteLine($"Successfully cast: {str}");
}

// Pattern matching with 'is' operator (C# 7+)
if (obj is string stringValue)
{
    Console.WriteLine($"Pattern matched: {stringValue}");
}

// Switch expressions with type patterns (C# 8+)
string GetTypeDescription(object value) => value switch
{
    int i => $"Integer: {i}",
    string s => $"String: {s}",
    double d => $"Double: {d}",
    bool b => $"Boolean: {b}",
    null => "Null value",
    _ => $"Unknown type: {value.GetType().Name}"
};

// Generic type conversion
public static T ConvertTo<T>(object value) where T : IConvertible
{
    return (T)Convert.ChangeType(value, typeof(T));
}

// Safe conversion with default values
public static T SafeConvert<T>(object value, T defaultValue = default) where T : struct
{
    try
    {
        return (T)Convert.ChangeType(value, typeof(T));
    }
    catch
    {
        return defaultValue;
    }
}

// Nullable conversion helpers
public static int? ToNullableInt(string value)
{
    return int.TryParse(value, out int result) ? result : null;
}

public static DateTime? ToNullableDateTime(string value)
{
    return DateTime.TryParse(value, out DateTime result) ? result : null;
}

// Custom conversion with IConvertible
public class Temperature : IConvertible
{
    private double celsius;
    
    public Temperature(double celsius) => this.celsius = celsius;
    
    public double ToCelsius() => celsius;
    public double ToFahrenheit() => (celsius * 9.0 / 5.0) + 32;
    public double ToKelvin() => celsius + 273.15;
    
    // IConvertible implementation
    public TypeCode GetTypeCode() => TypeCode.Object;
    public double ToDouble(IFormatProvider provider) => celsius;
    public string ToString(IFormatProvider provider) => $"{celsius}°C";
    // ... other IConvertible methods
}
```

## Error Handling in Conversions
```csharp
// Try-parse patterns for different types
public static class SafeConverter
{
    public static bool TryConvertToInt(string value, out int result)
    {
        return int.TryParse(value, out result);
    }
    
    public static bool TryConvertToDouble(string value, out double result)
    {
        return double.TryParse(value, NumberStyles.Float, CultureInfo.InvariantCulture, out result);
    }
    
    public static bool TryConvertToDateTime(string value, out DateTime result)
    {
        return DateTime.TryParse(value, out result);
    }
    
    public static bool TryConvertToEnum<T>(string value, out T result) where T : struct, Enum
    {
        return Enum.TryParse<T>(value, true, out result);
    }
    
    public static bool TryConvertToBool(string value, out bool result)
    {
        result = false;
        if (string.IsNullOrWhiteSpace(value)) return false;
        
        string normalized = value.Trim().ToLowerInvariant();
        if (normalized == "1" || normalized == "true" || normalized == "yes" || normalized == "on")
        {
            result = true;
            return true;
        }
        if (normalized == "0" || normalized == "false" || normalized == "no" || normalized == "off")
        {
            result = false;
            return true;
        }
        
        return bool.TryParse(value, out result);
    }
}

// Exception handling in conversions
public static class ConversionHelper
{
    public static T SafeConvert<T>(object value, T defaultValue, Action<Exception> onError = null)
    {
        try
        {
            if (value == null || value == DBNull.Value)
                return defaultValue;
                
            if (value is T directValue)
                return directValue;
                
            return (T)Convert.ChangeType(value, typeof(T));
        }
        catch (Exception ex)
        {
            onError?.Invoke(ex);
            return defaultValue;
        }
    }
    
    public static Result<T> TryConvert<T>(object value)
    {
        try
        {
            if (value == null || value == DBNull.Value)
                return Result<T>.Failure("Value is null");
                
            var converted = (T)Convert.ChangeType(value, typeof(T));
            return Result<T>.Success(converted);
        }
        catch (Exception ex)
        {
            return Result<T>.Failure(ex.Message);
        }
    }
}

// Result pattern for safe conversions
public class Result<T>
{
    public bool IsSuccess { get; private set; }
    public T Value { get; private set; }
    public string Error { get; private set; }
    
    private Result(bool isSuccess, T value, string error)
    {
        IsSuccess = isSuccess;
        Value = value;
        Error = error;
    }
    
    public static Result<T> Success(T value) => new Result<T>(true, value, null);
    public static Result<T> Failure(string error) => new Result<T>(false, default, error);
}
```

## Culture-Aware Conversions
```csharp
// Culture-sensitive parsing
CultureInfo germanCulture = CultureInfo.GetCultureInfo("de-DE");
CultureInfo usCulture = CultureInfo.GetCultureInfo("en-US");

// German uses comma as decimal separator
string germanNumber = "123,45";
double germanValue = double.Parse(germanNumber, germanCulture); // 123.45

// US uses period as decimal separator
string usNumber = "123.45";
double usValue = double.Parse(usNumber, usCulture); // 123.45

// Date parsing with different cultures
string germanDate = "25.12.2023";
DateTime date1 = DateTime.Parse(germanDate, germanCulture);

string usDate = "12/25/2023";
DateTime date2 = DateTime.Parse(usDate, usCulture);

// Invariant culture for data storage
string invariantNumber = (123.45).ToString(CultureInfo.InvariantCulture);
double parsed = double.Parse(invariantNumber, CultureInfo.InvariantCulture);

// Custom number formatting
NumberFormatInfo customFormat = new NumberFormatInfo
{
    NumberDecimalSeparator = ".",
    NumberGroupSeparator = " ",
    NumberGroupSizes = new int[] { 3 }
};

string formatted = (1234567.89).ToString("N2", customFormat); // "1 234 567.89"
```

## Performance Considerations
```csharp
// Comparison of conversion methods performance
[MemoryDiagnoser]
public class ConversionBenchmark
{
    private readonly string[] _stringNumbers = Enumerable.Range(1, 1000)
        .Select(i => i.ToString()).ToArray();

    [Benchmark]
    public int[] UsingParse()
    {
        return _stringNumbers.Select(int.Parse).ToArray();
    }

    [Benchmark]
    public int[] UsingConvert()
    {
        return _stringNumbers.Select(Convert.ToInt32).ToArray();
    }

    [Benchmark]
    public int[] UsingTryParse()
    {
        return _stringNumbers.Select(s => int.TryParse(s, out int result) ? result : 0).ToArray();
    }

    [Benchmark]
    public int[] UsingDirectCast()
    {
        object[] objects = _stringNumbers.Cast<object>().ToArray();
        return objects.Select(o => (int)Convert.ChangeType(o, typeof(int))).ToArray();
    }
}

// Optimized conversion for large datasets
public static class FastConverter
{
    // Pre-compiled regex for number validation
    private static readonly Regex NumberRegex = new Regex(@"^-?\d+$", RegexOptions.Compiled);
    
    public static int[] ConvertStringArrayToInt(string[] strings)
    {
        int[] result = new int[strings.Length];
        for (int i = 0; i < strings.Length; i++)
        {
            if (int.TryParse(strings[i], out int value))
                result[i] = value;
        }
        return result;
    }
    
    // Using Span<T> for memory-efficient conversions
    public static bool TryParseIntSpan(ReadOnlySpan<char> span, out int result)
    {
        return int.TryParse(span, out result);
    }
}
```

## JSON and XML Conversions
```csharp
// JSON conversion with System.Text.Json
using System.Text.Json;

public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public DateTime BirthDate { get; set; }
}

// Object to JSON
Person person = new Person { Name = "John", Age = 30, BirthDate = DateTime.Now };
string json = JsonSerializer.Serialize(person);

// JSON to Object
Person deserializedPerson = JsonSerializer.Deserialize<Person>(json);

// Custom JSON converter
public class DateTimeConverter : JsonConverter<DateTime>
{
    public override DateTime Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)
    {
        return DateTime.ParseExact(reader.GetString(), "yyyy-MM-dd", CultureInfo.InvariantCulture);
    }

    public override void Write(Utf8JsonWriter writer, DateTime value, JsonSerializerOptions options)
    {
        writer.WriteStringValue(value.ToString("yyyy-MM-dd"));
    }
}

// XML conversion
using System.Xml.Serialization;

[XmlRoot("Person")]
public class PersonXml
{
    [XmlElement("Name")]
    public string Name { get; set; }
    
    [XmlElement("Age")]
    public int Age { get; set; }
}

// Object to XML
XmlSerializer xmlSerializer = new XmlSerializer(typeof(PersonXml));
using StringWriter stringWriter = new StringWriter();
xmlSerializer.Serialize(stringWriter, person);
string xml = stringWriter.ToString();

// XML to Object
using StringReader stringReader = new StringReader(xml);
PersonXml deserializedFromXml = (PersonXml)xmlSerializer.Deserialize(stringReader);
```

## Database Type Conversions
```csharp
// Safe database value conversion
public static class DbConverter
{
    public static T GetValue<T>(object dbValue, T defaultValue = default)
    {
        if (dbValue == null || dbValue == DBNull.Value)
            return defaultValue;
            
        if (dbValue is T directValue)
            return directValue;
            
        try
        {
            return (T)Convert.ChangeType(dbValue, typeof(T));
        }
        catch
        {
            return defaultValue;
        }
    }
    
    public static string GetString(object dbValue) => GetValue<string>(dbValue, string.Empty);
    public static int GetInt32(object dbValue) => GetValue<int>(dbValue, 0);
    public static double GetDouble(object dbValue) => GetValue<double>(dbValue, 0.0);
    public static DateTime GetDateTime(object dbValue) => GetValue<DateTime>(dbValue, DateTime.MinValue);
    public static bool GetBoolean(object dbValue) => GetValue<bool>(dbValue, false);
    
    public static int? GetNullableInt32(object dbValue)
    {
        return dbValue == null || dbValue == DBNull.Value ? null : GetValue<int>(dbValue);
    }
    
    public static DateTime? GetNullableDateTime(object dbValue)
    {
        return dbValue == null || dbValue == DBNull.Value ? null : GetValue<DateTime>(dbValue);
    }
}

// Entity mapping with type conversion
public class EntityMapper<T> where T : new()
{
    public T MapFromDataReader(IDataReader reader)
    {
        T entity = new T();
        var properties = typeof(T).GetProperties();
        
        foreach (var prop in properties)
        {
            if (HasColumn(reader, prop.Name))
            {
                object value = reader[prop.Name];
                if (value != DBNull.Value)
                {
                    object convertedValue = Convert.ChangeType(value, prop.PropertyType);
                    prop.SetValue(entity, convertedValue);
                }
            }
        }
        
        return entity;
    }
    
    private bool HasColumn(IDataReader reader, string columnName)
    {
        for (int i = 0; i < reader.FieldCount; i++)
        {
            if (reader.GetName(i).Equals(columnName, StringComparison.OrdinalIgnoreCase))
                return true;
        }
        return false;
    }
}
```

## Additional Resources

### Official Documentation
- [Microsoft Type Conversion Documentation](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/casting-and-type-conversions) - Comprehensive guide to type conversions
- [Convert Class](https://docs.microsoft.com/en-us/dotnet/api/system.convert) - Complete Convert class reference
- [Implicit and Explicit Conversions](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/conversions) - Language specification for conversions
- [Checked and Unchecked Context](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/checked) - Overflow checking in conversions
- [Nullable Value Types](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/nullable-value-types) - Converting nullable types

### Types of Conversions
- [Boxing and Unboxing](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/boxing-and-unboxing) - Value type to object conversions
- [User-Defined Conversions](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/user-defined-conversion-operators) - Custom conversion operators
- [Implicit Numeric Conversions](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/numeric-conversions) - Safe numeric conversions
- [Explicit Numeric Conversions](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/numeric-conversions#explicit-numeric-conversions) - Conversions that may lose data

### Learning Resources
- **Books:**
  - "C# in Depth" by Jon Skeet - Advanced type system and conversions
  - "Effective C#" by Bill Wagner - Best practices for type conversions
  - "CLR via C#" by Jeffrey Richter - Deep dive into .NET type system
  - "Pro .NET Performance" by Sasha Goldshtein - Performance optimization of conversions

- **Online Courses:**
  - [Microsoft Learn Type Conversions](https://docs.microsoft.com/en-us/learn/modules/csharp-convert-cast/) - Interactive tutorials
  - [Pluralsight Working with Data Types](https://www.pluralsight.com/courses/csharp-fundamentals-dev) - Professional training

- **Practice Platforms:**
  - [LeetCode Data Conversion Problems](https://leetcode.com/tag/string/) - Algorithm challenges
  - [HackerRank Type Conversion](https://www.hackerrank.com/domains/algorithms) - Programming exercises
  - [Codewars Conversion Kata](https://www.codewars.com/kata/search/csharp?q=conversion) - Conversion practice

### Real-World Applications
- **Data Processing:** Converting data between different formats (CSV, JSON, XML)
- **User Input Validation:** Safely converting user input to appropriate types
- **Database Operations:** Converting between database types and .NET types
- **Configuration Management:** Parsing configuration values from strings
- **API Development:** Converting between DTOs and domain models
- **File Processing:** Reading and writing different data formats
- **Serialization:** Converting objects to/from various storage formats

### Community Resources
- [Stack Overflow Type Conversion](https://stackoverflow.com/questions/tagged/type-conversion+c%23) - Q&A community
- [C# Discord](https://discord.gg/csharp) - Real-time help and discussions
- [Reddit r/csharp](https://www.reddit.com/r/csharp/) - Community discussions
- [.NET Foundation](https://dotnetfoundation.org/) - Official .NET community
- [Microsoft Developer Community](https://developercommunity.visualstudio.com/) - Report issues and suggestions

### Tools and Libraries
- **NuGet Packages:**
  - [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/) - High-performance JSON serialization
  - [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) - Popular JSON framework
  - [AutoMapper](https://www.nuget.org/packages/AutoMapper/) - Object-to-object mapping
  - [CsvHelper](https://www.nuget.org/packages/CsvHelper/) - CSV reading and writing
  - [FluentValidation](https://www.nuget.org/packages/FluentValidation/) - Input validation

- **Development Tools:**
  - Visual Studio IntelliSense for type conversion suggestions
  - Code analysis tools for conversion safety (SonarQube, CodeMaid)
  - Profiling tools for conversion performance (dotTrace, PerfView)
  - Unit testing frameworks (NUnit, xUnit, MSTest)