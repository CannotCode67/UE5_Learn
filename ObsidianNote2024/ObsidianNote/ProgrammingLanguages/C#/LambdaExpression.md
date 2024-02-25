Remember we discuss how anonymous method syntax can save us from jumping out of the current context by allowing us to write a method definition inline. Lambda expression was brought into C# for a similar purpose, but lambda expression has a even more concise syntax which makes it perfect for working along side with LINQ.

The following is three different ways of defining a method. They are all used in the LINQ method `Where()`. This LINQ method can filter a collection with given crtieria. We are using methods as parameters, so yes, the `Where()` method takes in the built-in delegate `Predicate<T>` as parameter. Where do we get the T? Well, `Where()` works on collections that implement `IEnumerable<T>` interface, and that is where the method gets the value of T and passes it into the delegate. Also, as you may recall, `Predicate<T>` always return a boolean value, and if true, we keep the item, and if false, we don't keep it. That's the story of `Where()`, a part of it. We, then can pass a method into the `Where()` method as a criteria.
Standard way
```c#
static void Main(string[] args)
{
	IEnumerable<string> filteredList = cities.Where(StartsWithL);
}
public static bool StartsWithL(string name)
{
	return name.StartsWith("L");
}
```
Anonymous method way
```C#
IEnumerable<string> filteredList = cities.Where(delegate(string s)
											   {return s.StartsWith("L");});
```
Lambda expression way
```C#
IEnumerable<string> filteredList = cities.Where(s => s.StartsWith("L"));
```

We can clearly see the lambda way is by far the shortest, but it does that without losing any information. Let's see how. First, through definition of `Where()` method, C# knows the `Where()` method is expecting a delegate, so we can safely get rid of the keyword delegate. Next, we mention the `Predicate<T>` gets its T from `IEnumerable<T>` interface, so we know it must be taking in a string in this case, without the need to explicitly write it out. The `Predicate<T>` must return a bool, so the keyword return can be taken out. Now, all we have is `(s){s.StartsWith("L");}` inside `Where()` method. Lastly to make the syntax cleaner, we replace the parenthesis, curly braces, and semicolon with a goto operator `=>` like this, `s => s.StartsWith("L")`. There we have our lambda expression. As you can see, it leaves out no information, makes no assumption, it is just as equally good as the anonymous method way. Yet, it is even more concise, perhaps more readable to many developers.

That's the story of lambda expression, and it proves to be a great feature now that most developers would choose lambda expression over anonymous method syntax given opportunity. With that being said, they are suitable for short, simple methods.