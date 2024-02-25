First of all, LINQ stands for language intergrated query. It is a feature offered by C#, and it allows us to perform strongly-type, compile-time checks on query that would execute against in-memory data, relational data, and XML data. When we don't know where a piece of data comes from, we call it sequence.

That is the description of LINQ, and without LINQ, C# developers would need to use different sets of APIs to query data from different sources, one for XML, one of relational data, etc. Now, we just need to learn LINQ, and we'll be able to query a wide variety of data sources. Not all apparently, LINQ would only work on data sources that are designed to work with LINQ, or have APIs to additionally transform our LINQ into whatever language the data sources use to communicate.

To understand LINQ, we first need to understnad `IEnumerable<T>` interface. Yes, we study this interface before when we study generic interface. But there is more to learn, and for one thing, this interface is where LINQ is implemented on top of.

The definition of `IEnumerable<T>` is this. When we study generic interface, we said implementing the `IEnumerbale<T>` is one of the answsers to satisfy the requirement for a data struture to be able to get iterated through using a foreach statement. That requirement is the `IEnumerator<T>`.
```C#
public interface IEnumerable<out T> : IEnumerator
{
	IEnumerator<T> GetEnumerator();
}

public interface IEnumerable
{
	IEnumerator GetEnumerator();
}
```
The foreach statement is implicitly using the GetEnumerator() method. We can also explicitly invoke it, to get an `IEnumerator<T>` object. `IEnumerator<T>` is a data structure like any other collection, it stores items while allowing those items to be accessed through iteration. We can obviously see this because when we accesses enumertator, we first need to access its Current property, before we can reach the Employee object's Name property. The foreach statement is again a simplified syntax that under the hood utilizes the same data structure to achieve iteration.
```C#
IEnumerator<Employee> enumerator = developers.GetEnumerator();
while (enumerator.MoveNext())
{
	Console.WriteLine(enumerator.Current.Name);
}
foreach employee in employeeList
{
	Console.WriteLine(employee.Name);
}
```
How this GetEnumerator() method returns an `IEnumerator<T>` object? Because it is an interface, and we are responsible to write the implementation. Well, first we revisit the code we used in generic interface study. As you may recall, we said we will take a closer look at the keyword yield return. And the interesting part of this implementation is really just the yield return keyword. The rest is just iteration, and we happens to use a foreach loop because the underlying data structure `queue` has a built-in GetEnumerator() method, and we can use a for loop instead if that is not the case.
```C#
public IEnumerator<T> GetEnumerator()
{
	foreach (var item in thisQueue)
	{
		// ...do whatever we want
		yield return item;
	}
}
```
The implementation is taking each item in the queue and put it into a collection of `IEnumerator<T>`. If it is a regular collection, we probably first has a empty collection, then inside the foreach loop, we take the item out of the queue, and put the item into the collection using method similar to Add(). However, we don't see all that here. Instead, we just see the yield return keyword. The yield return keyword would tell the C# to turn it into `IEnumerator<T>`. You may ask why don't we just use the keyword new() then? Well, because C# treats `IEnumerator<T>` specially. The code inside this method is part of the definition of this `IEnumerator<T>`. And it would only get executed when someone tries to inspect the `IEnumerator<T>`, which is known as deferred execution. In previous example, we explicitly invoke the GetEnumerator() method to get a `IEnumerator<T>` object. But, the code we write in GetEnumerator() doesn't get executed, untill the while loop checks the condition by calling the MoveNext() method in the `IEnumerator<T>` object.
```C#
IEnumerator<Employee> enumerator = developers.GetEnumerator();
while (enumerator.MoveNext())
{
	Console.WriteLine(enumerator.Current.Name);
}
```
And the word yield in English means stop, and the lind of code `yield return item;` actually does similar thing, the yield keyword stops the execution in the GetEnumerator() method, then the `return item` returns the item in `IEnumerator<T>` format out of the method, into the place where code is inspecting the `IEnumerator<T>` object. After it is done, the execution goes back into the GetEnumerator() method to execute next iteration of the foreach loop. This behaviour is called streaming. These two distinguishing behaviours make `IEnumerator<T>` so different from other collections. 

We can write our own method to return `IEnumerator<T>`, the gist is the yield return keyword, not the method name. The following code is a custom extension method extending the `IEnumerable<T>` interface, but it would show no behaviour of deferred execution, or streaming, even though we can clearly see the return type is `IEnumerable<T>`. And the reason we don't get an compiler error is the result is a `List<T>` object which type implements `IEnumerable<T>`, so it can be treated as the type of `IEnumerable<T>`.  However, without the yield return keyword, it would not have those behaviours even we tell C# to treat it as `IEnumerable<T>`.
```C#
public static IEnumerable<T> Filter<T>(this IEnumerable<T> source, 
									   Func<T, bool> predicate)
{
	var result = new List<T>();
	foreach (var item in source)
	{
		if (predicate(item))
		{
			result.Add(item);
		}
	}
	return result;
}
```

Now that we fully understand `IEnumerable<T>` interface, you may ask what is the connection between all that and LINQ. LINQ, despite the fact it can do other things, is designed mainly for query, and what do we do with a query very often? Yes, we iterate through it. All the LINQ operators are methods, but if we implement all those methods in the `IEnumerable<T>` interface, we would make it so heavy, and any code that uses this interface before and after LINQ would be such a pain to maintain because each time a new LINQ oeprator is added, we need to rewrite the code. Therefore, LINQ operators are mostly extension methods for the `IEnumerable<T>` interface, like the custom Filter() method above. If we try to use that method in our previous example, it would show the look of a LINQ operator in action.
```C#
var query = developers.Filter(m => m.Name.Count() == 5);
foreach (var develop in query)
{
	Console.WriteLine(query.Name);
}
```
The result would show only the develops' names which are composed of 5 characters. Query is now an `IEnumerable<T>` object contains only the developers whose name is composed of 5 characters.

We have seen `Where()` method has deferred execution behaviour and streaming behaviour, but not all the LINQ operators do, some of them are immediately executed just like regular code, and some of them are deferred, but need to inspect all items to produce a result, so-non streaming. And LINQ operators and LINQ methods

LINQ methods can be categorized into three kinds, immediate execution, deferred streaming execution, and deferred non-streaming execution. Microsoft provides a classification table of LINQ operators for C#, that is where we can check which method belongs to what category. Typically, if a LINQ method returns an `IEnumerable<T>`, it probably belongs to the deferred execution category. If a LINQ method has a name starting with "To", and returns a concrete type like a List or an array, then it probably belongs to immediate execution category. Again, the classification table is there if you want to make sure of it.

For deferred execution, we know those LINQ methods must use yield return keyword. This deferred execution allows us to construct a query, but later the query might operate on different set of data upon other users' code. Because the code is not executed, there is no performance cost on it. Some methods like `Where()` would be able to stream data, and some methods like `OrderBy()` cannot. That's because we cannot put a collection into a specific order before checking all the items. But `OrderBy()` is still deferred, so the collection would not be in order before anyone inspects the collection. On the other hand, the streaming behaviour allows `Where()` method to terminate early if criteria is met. Like if we only need ten items that meet the criteria, then we don't waste resource iterating through the rest of the collection once we find them. Therefore, to make it efficient, we should try to use only the streaming operators, but once we introduce any non-streaming operator, we know it is gonna iterate through all the items. We can, however, separate the query into two parts, first we go through all the streaming operators, then we use what we get to go through non-streaming operators. Another thing to notice, if we use only streaming operators, the query now can work against a data source that is theoretically infinitely long, as long as we have one of the operators to end the query, like `Take()`, will return the number of items equaling to the number we pass in as parameter.

Deferred execution behaviour sometimes can lead to more work if not careful. That's when we put our immediate LINQ method before we invoke the deferred one, which would result in iterating through the collection twice. If that method order cannot be changed, we can turn the query into a List using LINQ method `ToList()`, then do what we need with that list. Another pitfall would be handling exceptions. We might use a try-catch block to wrap around the query, expecting an exception is thrown in that line of code, but if the code inspecting the query is only executed in later part of the program and throws an exception, then we would not catch it.