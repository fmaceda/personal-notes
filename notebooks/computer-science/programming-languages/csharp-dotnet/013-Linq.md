# Language Integrated Query (LINQ)

## Table of Content

1. [Overview](#overview)
2. [Using LINQ](#using-linq)
3. [In-memory vs Remote data](#in-memory-vs-remote-data)
4. [Parts of Query Operation](#parts-of-query-operation)
5. [Ending a Query Expression](#ending-a-query-expression)
6. [Filtering, ordering, and joining](#filtering-ordering-and-joining)
7. [Lambda Expressions](#lambda-expressions)
8. [Common LINQ Methods Reference](#common-linq-methods-reference)
9. [Additional Resources](#additional-resources)

## Overview

Language-Integrated Query (LINQ) is the name for a set of technologies based on the integration of query capabilities directly into the C# language. Traditionally, queries against data are expressed as simple strings without type checking at compile time or IntelliSense support. Furthermore, it's required to learn a different query language for each type of data source: SQL databases, XML documents, various Web services, and so on. With LINQ, a query is a first-class language construct, just like classes, methods, and events.

## Using LINQ

```csharp
// Specify the data source.
List<int> scores = [97, 92, 81, 60];
// Or
// int[] scores = [97, 92, 81, 60];

// Define the QUERY SYNTAX expression.
IEnumerable<int> scoreQuery1 =
  from score in scores // required
  where score > 80 // optional
  orderby score descending // optional
  select score; // must end with select or group

// Or METHOD SYNTAX
IEnumerable<int> scoreQuery2 = scores
  .Where(s => s > 80) // lambda expression
  .OrderByDescending(s => s); // lambda expression

// Note: Query and method syntaxes can be used together.
var scoreQuery3 = (
  from score in scores
  where score > 80
  orderby score descending
  select score
).Count();

// Execute the query.
foreach (var i in scoreQuery1)
{
  Console.Write(i + " ");
}
```

## In-memory vs Remote data

There are two ways you enable LINQ querying of in-memory data. If the data is of a type that implements IEnumerable<T>, you query the data by using LINQ to Objects.

```csharp
List<int> scores = [97, 92, 81, 60];

// In-memory
IEnumerable<int> scoreQuery1 =
  from score in scores
  where score > 80
  orderby score descending
  select score;

// Remote - It can be integrated with libraries like Entity Framework.
// You first create an object-relational mapping between C# classes and your database schema.
// Then you write your queries against the objects
// At run-time EntityFramework handles the communication with the database.
// It will even be able to build the SQL query and execute it in the database side.
class MyORM
{
  public IQueryable<int> GetQueryableProducts()
  {
    // This method would typically return a queryable collection of products from a database.
    // For demonstration, we return an empty queryable.
    return Enumerable.Empty<int>().AsQueryable();
  }
}

var myORM = new MyORM();

IQueryable<int> scoreQuery2 = myORM.GetQueryableProducts();
```

## Parts of Query Operation

ll LINQ query operations consist of three distinct actions:

1. Obtain the data source.
2. Create the query.
3. Execute the query.

```csharp
// 1. Data source.
int[] numbers = [ 0, 1, 2, 3, 4, 5, 6 ];

// 2. Query creation.
// numQuery is an IEnumerable<int>
var numQuery = from num in numbers
               where (num % 2) == 0
               select num;

// 3. Query execution.
foreach (int num in numQuery)
{
  Console.Write("{0,1} ", num);
}
```

## Ending a Query Expression

```csharp
class Country
{
  public string Name { get; set; }
  public long Population { get; set; }
  public double Area { get; set; } // in square kilometers
}

var countries = new List<Country>
{
  new Country { Name = "United States", Population = 331_000_000, Area = 9_525_067 },
  new Country { Name = "Canada", Population = 38_000_000, Area = 9_984_670 },
  new Country { Name = "Mexico", Population = 128_000_000, Area = 1_964_375 },
  new Country { Name = "Brazil", Population = 213_000_000, Area = 8_515_767 },
  new Country { Name = "Argentina", Population = 45_000_000, Area = 2_780_400 }
};

// GROUP CLAUSE
// Use the group clause to produce a sequence of groups organized by a key that you specify.
var queryCountryGroups =
  from country in countries
  group country by country.Name[0];

// SELECT CLAUSE
// Use the select clause to produce all other types of sequences.
IEnumerable<Country> sortedQuery =
  from country in countries
  orderby country.Area
  select country;
// The select clause can be used to transform source data into sequences of new types. 
var queryNameAndPop =
  from country in countries
  select new
  {
      Name = country.Name,
      Pop = country.Population
  };

// CONTINUATIONS WITH INTO
// You can use the into keyword in a select or group clause to create a temporary identifier that stores a query. 
// percentileQuery is an IEnumerable<IGrouping<int, Country>>
var percentileQuery =
  from country in countries
  let percentile = (int)country.Population / 1_000
  group country by percentile into countryGroup
  where countryGroup.Key >= 20
  orderby countryGroup.Key
  select countryGroup;

// grouping is an IGrouping<int, Country>
foreach (var grouping in percentileQuery)
{
  Console.WriteLine(grouping.Key);
  foreach (var country in grouping)
  {
    Console.WriteLine(country.Name + ":" + country.Population);
  }
}
```

## Filtering, ordering, and joining

Between the starting from clause, and the ending select or group clause, all other clauses (where, join, orderby, from, let) are optional. Any of the optional clauses might be used zero times or multiple times in a query body.

```csharp
class City {
  public string Name { get; set; }
  public long Population { get; set; } // in people
}

class Country
{
  public string Name { get; set; }
  public long Population { get; set; }
  public double Area { get; set; } // in square kilometers
}

var cities = new List<City>
{
  new City { Name = "Tokyo", Population = 37_833_000 },
  new City { Name = "Delhi", Population = 31_000_000 },
  new City { Name = "Shanghai", Population = 24_150_000 },
  new City { Name = "São Paulo", Population = 22_046_000 },
  new City { Name = "Mexico City", Population = 21_782_000 }
};

var countries = new List<Country>
{
  new Country { Name = "United States", Population = 331_000_000, Area = 9_525_067 },
  new Country { Name = "Canada", Population = 38_000_000, Area = 9_984_670 },
  new Country { Name = "Mexico", Population = 128_000_000, Area = 1_964_375 },
  new Country { Name = "Brazil", Population = 213_000_000, Area = 8_515_767 },
  new Country { Name = "Argentina", Population = 45_000_000, Area = 2_780_400 }
};

var categories = new List<string> { "Electronics", "Clothing", "Books" };
var products = new List<(string Name, string Category)>
{
  ("Smartphone", "Electronics"),
  ("Laptop", "Electronics"),
  ("T-shirt", "Clothing"),
  ("Novel", "Books"),
  ("Headphones", "Electronics")
};
var students = new List<(string Name, int Year, List<int> ExamScores)>
{
  ("Alice", 1, [85, 90, 78]),
  ("Bob", 2, [88, 92, 95]),
  ("Charlie", 1, [80, 85, 82]),
  ("David", 2, [90, 91, 89])
};
// WHERE CLAUSE
// Use the where clause to filter out elements from the source data based on one or more predicate expressions. 
IEnumerable<City> queryCityPop =
  from city in cities
  where city.Population is < 15_000_000 and > 10_000_000
  select city;

// ORDERBY CLAUSE 
// Use the orderby clause to sort the results in either ascending or descending order. 
IEnumerable<Country> querySortedCountries =
  from country in countries
  orderby country.Area, country.Population descending
  select country;

// JOIN CLAUSE
// Use the join clause to associate and/or combine elements from one data source with elements from another data source based on an equality comparison between specified keys in each element. 
var categoryQuery =
  from cat in categories
  join prod in products on cat equals prod.Category
  select new
  {
    Category = cat,
    Name = prod.Name
  };

// LET CLAUSE
// Use the let clause to store the result of an expression, such as a method call, in a new range variable.
string[] names = ["Svetlana Omelchenko", "Claire O'Donnell", "Sven Mortensen", "Cesar Garcia"];
IEnumerable<string> queryFirstNames =
  from name in names
  let firstName = name.Split(' ')[0]
  select firstName;

foreach (var s in queryFirstNames)
{
  Console.Write(s + " ");
}

//Output: Svetlana Claire Sven Cesar

// SUBQUERIES IN A QUERY EXPRESSION
// A query clause might itself contain a query expression, which is sometimes referred to as a subquery.
var queryGroupMax =
  from student in students
  group student by student.Year into studentGroup
  select new
  {
    Level = studentGroup.Key,
    HighestScore = (
        from student2 in studentGroup
        select student2.ExamScores.Average()
    ).Max()
  };
```

## Lambda Expressions

```csharp
List<(string Name, int score)> scores = [
  ("Alice", 100),
  ("Bob", 45),
  ("Charlie", 75),
  ("David", 80),
  ("Eve", 90),
  ("Frank", 60),
  ("Grace", 70),
  ("Heidi", 85)
];

scores.Where(score => score.score > 80) // Lambda expression
      .OrderByDescending(score => score.score) // Lambda expression
      .ToList()
      .ForEach(score => Console.WriteLine($"{score.Name}: {score.score}")); // Lambda expression
```

## Common LINQ Methods Reference
```csharp
// Filtering
Where(), OfType<T>()

// Projection
Select(), SelectMany()

// Ordering
OrderBy(), OrderByDescending(), ThenBy(), ThenByDescending(), Reverse()

// Grouping
GroupBy(), ToLookup()

// Joining
Join(), GroupJoin()

// Aggregation
Count(), Sum(), Average(), Min(), Max(), Aggregate()

// Quantifiers
All(), Any(), Contains()

// Element Operations
First(), FirstOrDefault(), Last(), LastOrDefault(), Single(), SingleOrDefault(), ElementAt()

// Set Operations
Distinct(), Union(), Intersect(), Except()

// Conversion
ToArray(), ToList(), ToDictionary(), ToLookup(), AsEnumerable(), AsQueryable()
```

## Additional Resources

### Official Documentation
- [Microsoft LINQ Documentation](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/) - Comprehensive guide to LINQ concepts and examples
- [LINQ Query Syntax vs Method Syntax](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) - Detailed comparison of query and method syntaxes
- [Standard Query Operators Overview](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/standard-query-operators-overview) - Complete reference for LINQ operators

### LINQ Providers
- **LINQ to Objects** - For in-memory collections (IEnumerable<T>)
- **LINQ to SQL** - For SQL Server databases (legacy, use Entity Framework instead)
- **LINQ to XML** - For XML document manipulation
- **Entity Framework Core** - Modern ORM with LINQ support for databases
- **LINQ to JSON** - For JSON data manipulation (Newtonsoft.Json)

### Performance Considerations
- Use `IQueryable<T>` for database queries to enable server-side filtering
- Consider using `AsParallel()` for CPU-intensive operations on large datasets
- Be aware of deferred execution and when queries are actually executed

### Popular LINQ Extension Libraries
- **MoreLINQ** - Additional LINQ operators not in the standard library
- **System.Linq.Async** - LINQ support for async/await scenarios
- **System.Interactive** - Interactive Extensions (Ix) for additional operators

### Learning Resources
- [LINQPad](https://www.linqpad.net/) - Interactive tool for testing and learning LINQ queries
- [C# in Depth by Jon Skeet](https://csharpindepth.com/) - Detailed coverage of LINQ internals and advanced concepts