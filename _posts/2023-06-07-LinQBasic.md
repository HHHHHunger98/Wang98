---
layout: post
title: LinQBasic
date: 2023-06-07
tags: csharp
---

# LINQ in C#

## What is LINQ?

> LINQ stands for Language Integrated Query 

## Why we need LINQ?

> A query is an expression written in a specific query language that retrieves data from a data source, but there can be a variety of query language. And for different data source the developers need to learn a new query language, That is not a efficient way. So LINQ aims to solve this problem by providing a consistent model for working with data source across various kinds of data sources and formats.

> In a LINQ query, you are always working with objects. You use the same basic coding patterns to query and transform data in XML documents, SQL databases, ADO.NET Datasets, .NET collections, and any other format for which a LINQ provider is available.

## Three parts of a Query Operation

1. data source
2. the query itself
3. execution of the query

for example:
```C#
class IntroToLINQ
{
    static void Main()
    {
        // The Three Parts of a LINQ Query:
        // 1. Data source.
        int[] numbers = new int[7] { 0, 1, 2, 3, 4, 5, 6 };

        // 2. Query creation.
        // numQuery is an IEnumerable<int>
        var numQuery =
            from num in numbers
            where (num % 2) == 0
            select num;

        // 3. Query execution.
        foreach (int num in numQuery)
        {
            Console.Write("{0,1} ", num);
        }
    }
}
```

> remarkable: the query itself does not retrieved any data, it is just a variable

## What kinds of data source does LINQ support

1. **All data that supports the generic `IEnumerable<T>` interface or a derived interface such as the generic `IQueryable<T>` can be queried with LINQ, they are called <span style="background-color: #FFFF00"> queryable type </span>**
2. If the data is not already in memory as a queryable type, LINQ provider needs to represent it as such a queryable data type, for example LINQ to XML loads an XML document into a queryable `XElement` type.
    ```C#
    // Create a data source from an XML document.
    // using System.Xml.Linq;
    XElement contacts = XElement.Load(@"c:\myContactList.xml");
    ```
> <span style="background-color: #FFFF00"> Basic Rule: A LINQ data source is any object that supports the generic `IEnumerable<T>` interface!!!!! </span>

## The Query

> C sharp introduces a new query syntax, the query is stored in a query variable and initialized with a query expression

**A query expression contains three clauses: `from` `where` `select`**
1. from: indicating the data source
2. where: the filter used in this query
3. select: specifies the type of the returned elements

## Query execution

1. deferred execution: the query is stored in a variable, the actual retrieval of data is deferred until you iterate over the query variable in a `foreach` statement.
2. there are also ways to execute the query immediately: using aggregation functions likes `Count` `Max` `Average` and `First`. Or cache the query results in a `List` object using `ToList()` or `ToArray()` methods
    ```c#
    var evenNumQuery =
    from num in numbers
    where (num % 2) == 0
    select num;

    int evenNumCount = evenNumQuery.Count();
    List<int> numQuery2 =
    (from num in numbers
     where (num % 2) == 0
     select num).ToList();

    // or like this:
    // numQuery3 is still an int[]

    var numQuery3 =
    (from num in numbers
     where (num % 2) == 0
     select num).ToArray();
    ```
