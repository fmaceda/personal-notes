# Strings

## Table of Contents

1. [What is a String?](#what-is-a-string)
2. [String Methods and Properties](#string-methods-and-properties)
   - [Length](#length)
   - [IndexOf](#indexof)
   - [LastIndexOf](#lastindexof)
   - [IndexOfAny](#indexofany)
   - [Remove](#remove)
   - [Substring](#substring)
   - [Replace](#replace)
   - [Trim](#trim)
   - [Case](#case)
   - [Contains, StartsWith and EndsWith](#contains-startswith-and-endswith)
3. [Modern C# String Features](#modern-c-string-features)
4. [String Performance and Best Practices](#string-performance-and-best-practices)
5. [Regular Expressions with Strings](#regular-expressions-with-strings)
6. [String Formatting and Localization](#string-formatting-and-localization)
7. [String Validation and Sanitization](#string-validation-and-sanitization)
8. [String Parsing and Conversion](#string-parsing-and-conversion)
9. [Performance Testing](#performance-testing)
10. [Additional Resources](#additional-resources)

## What is a String?

A string is a sequence of characters.

## String Methods and Properties 

### Length

Length is a property of a string and it returns the number of characters in that string.

```csharp
string message = "Find what is (inside the parentheses)";

Console.WriteLine(message.Length );
```

### IndexOf

IndexOf is a method that returns the index of the start of substring being searched.

```csharp
string message = "Find what is (inside the parentheses)";

int openingPosition = message.IndexOf('(');
int closingPosition = message.IndexOf(')');

Console.WriteLine(openingPosition);
Console.WriteLine(closingPosition);
```

### LastIndexOf

LastIndexOf is a method that returns the last occurrence of the index of substring being searched.

```csharp
string message = "hello there!";

int first_h = message.IndexOf('h');
int last_h = message.LastIndexOf('h');
Console.WriteLine($"For the message: '{message}', the first 'h' is at position {first_h} and the last 'h' is at position {last_h}."); 
```

### IndexOfAny

LastIndexOf is a method similar than IndexOf, but it receives an array as input, and searches the index of any of the substrings in the array.

```csharp
string message = "Hello, world!";
char[] charsToFind = { 'a', 'e', 'i' };

int index = message.IndexOfAny(charsToFind);

Console.WriteLine($"Found '{message[index]}' in '{message}' at index: {index}.");
```

### Remove

Remove is a method that removes a substring belonging to a big string, given the index of the start of the string and the number of characters to delete.

```csharp
string data = "12345John Smith          5000  3  ";
string updatedData = data.Remove(5, 20);
Console.WriteLine(updatedData);
```

### Substring

Substring is a method that returns a substring, given the index and the length of the substring within the big string.

```csharp
string message = "What is the value <span>between the tags</span>?";

const string openSpan = "<span>";
const string closeSpan = "</span>";

int openingPosition = message.IndexOf(openSpan);
int closingPosition = message.IndexOf(closeSpan);

openingPosition += openSpan.Length;
int length = closingPosition - openingPosition;
Console.WriteLine(message.Substring(openingPosition, length));
```

### Replace

Replace is a method that replaces a substring with other substring inside a big string.

```csharp
string message = "This--is--ex-amp-le--da-ta";
message = message.Replace("--", " ");
message = message.Replace("-", "");
Console.WriteLine(message);
```

### Trim

Trim, TrimStart and TrimEnd are methods that removes spaces between a string.

```csharp
string greeting = "      Hello World!       ";
Console.WriteLine($"[{greeting}]");

string trimmedGreeting = greeting.TrimStart();
Console.WriteLine($"[{trimmedGreeting}]");

trimmedGreeting = greeting.TrimEnd();
Console.WriteLine($"[{trimmedGreeting}]");

trimmedGreeting = greeting.Trim();
Console.WriteLine($"[{trimmedGreeting}]");
```

### Case

ToUpper and ToLower are methods to convert string to upper or lower cases.

```csharp
string songLyrics = "You say goodbye, and I say hello";
Console.WriteLine(songLyrics.ToUpper());
Console.WriteLine(songLyrics.ToLower());
```

### Contains, StartsWith and EndsWith

Contains StartsWith and EndsWith are methods that help searching specific substring inside a big string.

```csharp
string songLyrics = "You say goodbye, and I say hello";

Console.WriteLine(songLyrics.Contains("goodbye"));
Console.WriteLine(songLyrics.Contains("greetings"));

Console.WriteLine(songLyrics.StartsWith("You")); // true
Console.WriteLine(songLyrics.EndsWith("hello")); // true
```

## Modern C# String Features
```csharp
// String interpolation (C# 6+)
string name = "John";
int age = 25;
string message = $"Hello {name}, you are {age} years old!";

// Raw string literals (C# 11)
string json = """
{
    "name": "John",
    "age": 25,
    "city": "New York"
}
""";

// String interpolation with expressions
string formattedPrice = $"Price: {price:C2}"; // Currency format
string formattedDate = $"Date: {DateTime.Now:yyyy-MM-dd}";

// Verbatim strings for file paths
string filePath = @"C:\Users\Documents\file.txt";

// Pattern matching with strings (C# 8+)
string GetMessageType(string message) => message switch
{
    string s when s.StartsWith("ERROR") => "Error Message",
    string s when s.StartsWith("WARN") => "Warning Message",
    string s when s.StartsWith("INFO") => "Information Message",
    _ => "Unknown Message Type"
};

// Null-conditional operators with strings
string text = null;
int? length = text?.Length; // Returns null instead of throwing exception
string upper = text?.ToUpper() ?? "DEFAULT";

// String comparison with StringComparison
bool isEqual = string.Equals(str1, str2, StringComparison.OrdinalIgnoreCase);
bool contains = text.Contains("search", StringComparison.InvariantCultureIgnoreCase);

// Span<char> for high-performance string operations (C# 7.2+)
ReadOnlySpan<char> span = "Hello World".AsSpan();
ReadOnlySpan<char> slice = span.Slice(0, 5); // "Hello"

// String.Create for efficient string building
string result = string.Create(10, (value: 42, text: "Answer"), 
    (span, state) => $"The {state.text} is {state.value}".AsSpan().CopyTo(span));
```

## String Performance and Best Practices
```csharp
// Use StringBuilder for multiple concatenations
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++)
{
    sb.AppendLine($"Line {i}");
}
string result = sb.ToString();

// String pooling and interning
string interned = string.Intern("frequently used string");
bool isInterned = ReferenceEquals(str1, string.Intern(str1));

// Efficient string splitting
string[] parts = text.Split(',', StringSplitOptions.RemoveEmptyEntries);

// String joining
string[] items = { "apple", "banana", "cherry" };
string joined = string.Join(", ", items);

// Memory-efficient string operations with Span<T>
public static bool StartsWithFast(ReadOnlySpan<char> text, ReadOnlySpan<char> prefix)
{
    return text.StartsWith(prefix, StringComparison.Ordinal);
}

// Avoiding string allocations with string.Create
public static string FormatNumber(int number, int padding)
{
    return string.Create(padding, number, (span, n) =>
    {
        n.TryFormat(span, out int written);
        span.Slice(written).Fill('0');
    });
}

// Culture-aware string operations
CultureInfo culture = CultureInfo.GetCultureInfo("en-US");
string upperInvariant = text.ToUpperInvariant(); // Culture-independent
string upperCulture = text.ToUpper(culture); // Culture-specific

// Safe string operations
public static string SafeSubstring(string input, int startIndex, int length)
{
    if (string.IsNullOrEmpty(input) || startIndex < 0 || startIndex >= input.Length)
        return string.Empty;
    
    int actualLength = Math.Min(length, input.Length - startIndex);
    return input.Substring(startIndex, actualLength);
}
```

## Regular Expressions with Strings
```csharp
using System.Text.RegularExpressions;

// Email validation
string emailPattern = @"^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$";
bool isValidEmail = Regex.IsMatch(email, emailPattern);

// Extract numbers from text
string text = "The price is $123.45 and quantity is 10";
MatchCollection matches = Regex.Matches(text, @"\d+(?:\.\d+)?");
foreach (Match match in matches)
{
    Console.WriteLine($"Found number: {match.Value}");
}

// Replace patterns
string result = Regex.Replace(input, @"\s+", " "); // Replace multiple spaces with single space

// Named groups
string pattern = @"(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})";
Match match = Regex.Match("2023-12-25", pattern);
if (match.Success)
{
    string year = match.Groups["year"].Value;
    string month = match.Groups["month"].Value;
    string day = match.Groups["day"].Value;
}

// Compiled regex for performance
private static readonly Regex EmailRegex = new Regex(
    @"^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$", 
    RegexOptions.Compiled | RegexOptions.IgnoreCase);

public static bool IsValidEmail(string email) => EmailRegex.IsMatch(email);
```

## String Formatting and Localization
```csharp
// Standard numeric formats
decimal price = 1234.56m;
string currency = price.ToString("C"); // $1,234.56
string percentage = 0.85.ToString("P"); // 85.00%
string scientific = 1234.56.ToString("E"); // 1.234560E+003

// Custom numeric formats
string custom = price.ToString("#,##0.00"); // 1,234.56
string padded = 42.ToString("D5"); // 00042

// Date and time formatting
DateTime now = DateTime.Now;
string shortDate = now.ToString("d"); // 12/25/2023
string longDate = now.ToString("D"); // Monday, December 25, 2023
string custom = now.ToString("yyyy-MM-dd HH:mm:ss");

// Composite formatting
string message = string.Format("Hello {0}, today is {1:d}", name, DateTime.Now);

// Culture-specific formatting
CultureInfo french = CultureInfo.GetCultureInfo("fr-FR");
string frenchPrice = price.ToString("C", french); // 1 234,56 €

// Resource strings for localization
ResourceManager rm = new ResourceManager("MyApp.Resources", Assembly.GetExecutingAssembly());
string localizedMessage = rm.GetString("WelcomeMessage", CultureInfo.CurrentCulture);
```

## String Validation and Sanitization
```csharp
// Input validation
public static class StringValidator
{
    public static bool IsNullOrWhiteSpace(string value) => string.IsNullOrWhiteSpace(value);
    
    public static bool HasMinLength(string value, int minLength) 
        => !string.IsNullOrEmpty(value) && value.Length >= minLength;
    
    public static bool IsAlphaNumeric(string value) 
        => !string.IsNullOrEmpty(value) && value.All(char.IsLetterOrDigit);
    
    public static bool IsValidUrl(string url)
    {
        return Uri.TryCreate(url, UriKind.Absolute, out Uri result) 
            && (result.Scheme == Uri.UriSchemeHttp || result.Scheme == Uri.UriSchemeHttps);
    }
}

// String sanitization
public static class StringSanitizer
{
    public static string RemoveSpecialCharacters(string input)
    {
        return Regex.Replace(input, "[^a-zA-Z0-9_]", "");
    }
    
    public static string HtmlEncode(string input)
    {
        return System.Web.HttpUtility.HtmlEncode(input);
    }
    
    public static string TruncateWithEllipsis(string input, int maxLength)
    {
        if (string.IsNullOrEmpty(input) || input.Length <= maxLength)
            return input;
        
        return input.Substring(0, maxLength - 3) + "...";
    }
}

// Safe string operations
public static string SafeTrim(string input) => input?.Trim() ?? string.Empty;
public static string SafeToUpper(string input) => input?.ToUpper() ?? string.Empty;
public static string SafeReplace(string input, string oldValue, string newValue)
    => string.IsNullOrEmpty(input) ? string.Empty : input.Replace(oldValue, newValue);
```

## String Parsing and Conversion
```csharp
// Safe parsing
public static class SafeParser
{
    public static int ParseInt(string value, int defaultValue = 0)
    {
        return int.TryParse(value, out int result) ? result : defaultValue;
    }
    
    public static DateTime ParseDate(string value, DateTime defaultValue = default)
    {
        return DateTime.TryParse(value, out DateTime result) ? result : defaultValue;
    }
    
    public static T ParseEnum<T>(string value, T defaultValue = default) where T : struct, Enum
    {
        return Enum.TryParse<T>(value, true, out T result) ? result : defaultValue;
    }
}

// String to various types
string numberStr = "123";
int number = Convert.ToInt32(numberStr);
double doubleVal = Convert.ToDouble("123.45");
bool boolVal = Convert.ToBoolean("true");

// Parse with culture
string dateStr = "25/12/2023";
DateTime date = DateTime.ParseExact(dateStr, "dd/MM/yyyy", CultureInfo.InvariantCulture);

// Base64 encoding/decoding
string original = "Hello World";
string encoded = Convert.ToBase64String(Encoding.UTF8.GetBytes(original));
string decoded = Encoding.UTF8.GetString(Convert.FromBase64String(encoded));
```

## Performance Testing
```csharp
// Benchmark string operations
[MemoryDiagnoser]
public class StringPerformanceBenchmark
{
    private readonly string[] _words = Enumerable.Range(1, 1000)
        .Select(i => $"Word{i}").ToArray();

    [Benchmark]
    public string ConcatenateWithPlus()
    {
        string result = "";
        foreach (string word in _words)
            result += word + " ";
        return result;
    }

    [Benchmark]
    public string ConcatenateWithStringBuilder()
    {
        var sb = new StringBuilder();
        foreach (string word in _words)
            sb.Append(word).Append(" ");
        return sb.ToString();
    }

    [Benchmark]
    public string ConcatenateWithJoin()
    {
        return string.Join(" ", _words);
    }

    [Benchmark]
    public string InterpolateStrings()
    {
        return string.Join(" ", _words.Select(w => $"{w}"));
    }
}
```

## Additional Resources

### Official Documentation
- [Microsoft String Class Documentation](https://docs.microsoft.com/en-us/dotnet/api/system.string) - Complete String class reference
- [String Methods Reference](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/strings/) - C# string programming guide
- [String Interpolation](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/tokens/interpolated) - Modern string formatting techniques
- [StringBuilder Class](https://docs.microsoft.com/en-us/dotnet/api/system.text.stringbuilder) - Efficient string building for multiple operations
- [String Comparison](https://docs.microsoft.com/en-us/dotnet/standard/base-types/best-practices-strings) - Best practices for string comparison

### Advanced String Operations
- [Regular Expressions](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expressions) - Pattern matching and text processing
- [String Encoding](https://docs.microsoft.com/en-us/dotnet/api/system.text.encoding) - Character encoding and Unicode handling
- [Globalization and Localization](https://docs.microsoft.com/en-us/dotnet/standard/globalization-localization/) - Culture-aware string operations
- [String Performance](https://docs.microsoft.com/en-us/dotnet/standard/base-types/stringbuilder) - Optimizing string operations

### Learning Resources
- **Books:**
  - "C# in Depth" by Jon Skeet - Advanced string handling techniques
  - "Effective C#" by Bill Wagner - String performance best practices
  - "Programming C# 10" by Ian Griffiths - Modern C# string features
  - "Clean Code" by Robert C. Martin - String naming and formatting principles

- **Online Courses:**
  - [Microsoft Learn C# Strings](https://docs.microsoft.com/en-us/learn/modules/csharp-basic-formatting/) - Interactive tutorials
  - [Coursera Text Processing](https://www.coursera.org/learn/algorithms-part2) - Advanced text algorithms

- **Practice Platforms:**
  - [LeetCode String Problems](https://leetcode.com/tag/string/) - Algorithm challenges
  - [HackerRank String Challenges](https://www.hackerrank.com/domains/algorithms?filters%5Bsubdomains%5D%5B%5D=strings) - Programming exercises
  - [Codewars String Kata](https://www.codewars.com/kata/search/csharp?q=string) - String manipulation practice

### Real-World Applications
- **Data Validation:** Email, phone number, and input validation
- **Text Processing:** Log file analysis, data extraction, content parsing
- **Configuration Management:** Reading configuration files, environment variables
- **Localization:** Multi-language applications, culture-specific formatting
- **Web Development:** URL manipulation, HTML sanitization, JSON processing
- **Security:** Password validation, SQL injection prevention, XSS protection
- **File Processing:** CSV parsing, text file manipulation, path handling

### Community Resources
- [Stack Overflow String Questions](https://stackoverflow.com/questions/tagged/string+c%23) - Q&A community
- [C# Discord](https://discord.gg/csharp) - Real-time help and discussions
- [Reddit r/csharp](https://www.reddit.com/r/csharp/) - Community discussions
- [.NET Foundation](https://dotnetfoundation.org/) - Official .NET community
- [GitHub Awesome .NET](https://github.com/quozd/awesome-dotnet) - Curated resources

### Tools and Libraries
- **NuGet Packages:**
  - [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/) - High-performance JSON processing
  - [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) - Popular JSON framework
  - [CsvHelper](https://www.nuget.org/packages/CsvHelper/) - CSV reading and writing
  - [FluentValidation](https://www.nuget.org/packages/FluentValidation/) - Fluent validation library
  - [HtmlAgilityPack](https://www.nuget.org/packages/HtmlAgilityPack/) - HTML parsing

- **Development Tools:**
  - Visual Studio IntelliSense for string methods
  - Regex testing tools (RegExr, RegexBuddy)
  - String interpolation IntelliSense
  - Code analysis for string performance (SonarQube)