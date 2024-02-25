When we say generic methods, it is very easy to make the connection to the method member of a generics class or a generics interface. So mayby something like this. The Add() method takes in a value that is of type T, and type T is defined when an object is created.

```C#
public class Buffer<T>
{
	private Queue<T> thisQueue
	public Buffer ()
	{
		thisQueue = new Queue<T>();
	}
	public void Add(T value)
	{
		thisQueue.Enqueue(value);
	}
}
```

There is nothing wrong with that, and they are generics in some sense. However, the term generic method usually refers to methods that still have room to adopt, room for maneuver, even after the object is created, so the T variable is defined already. 

Here we have the AsEnumerableOf<>() method, takes in a type parameter, not an object with that type, but just the type. When this method is called, we loop through our collection to convert each element from type of T, to type of TOutput. We get the T when the object is created, but we only get the TOutput when someone calls this method and passing the type parameter in the angle bracket. So, this method is generic even after the object is created, and if we give it different type parameter, we would get output of different type. It sounds like a method overload technique on top of a generic class, but it can only adop changes in type, not entire implementation. And accepting type as parameter in angle bracket is nothing like taking regualr parameter like value or object in the parentheses. This whole new idea of passing in a type as a parameter is important for us to understand another technique of C# called reflection.

```c#
public class Buffer<T>
{
	private Queue<T> thisQueue
	public Buffer ()
	{
		thisQueue = new Queue<T>();
	}
	public void Add(T value)
	{
		thisQueue.Enqueue(value);
	}
	public IEnumerable<TOutput> AsEnumerableOf<TOutput>()
	{
		var converter = TypeDescriptor.GetConverter(typeof(T));
		foreach (var item in thisQueue)
		{
			var result = converter.ConverTo(item, typeof(TOutput));
			yield return (TOutput)result;
		}
	}
}
```

Notice the converter we get from TypeDescriptor.GetConverter() mainly works for primitive types. And you might suspect that if none of the implementation requires the T variable, then can this method live inside of a regular class? Well, the answer is yes. That's why we need to separate the method members of a generic classs and generic methods.

If for whatever purpose, we want this generic method to be a extension method. Extension methods are special in a way that it can be invoked through object, even they are static methods. One of the information I give to the C# compiler is which this extension method is for, so the C# compiler knows the T in the Buffer<> class. And it is smart enough to share the T to the extension method, but to a very limited degree. None of the implementation would have the access of T. Here because the implementation doesn't require T, so it is fine.

```C#
public static class BufferExtensions
{   
	public static void Dump<T>(this Buffer<T> buffer)
	{
		foreach (var item in buffer)
		{
			Console.WriteLine(item);
		}
	}
}
```

C# knows the Dump<>() is an extension method for buffer object, and the object has double for T, so when we call Dump<>(), we can just write Dump() without specifying the T, C# compiler knows the T for Dump<>() is double as well. If we explicitly invoke the method using different type for T, like int in the angle bracket, we get a compiler error.

```C#
var buffer = new Buffer<double>();
ProcessInput(buffer);
buffer.Dump();
```

But like said, C# compiler shares that information to a very limited degree. In the case of AsEnumerableOf<>(), the implementation requires T, but has no access to it. The only way we can still receive information for T is we tell the method to get it from a pass-in type parameter. So the method now takes in two type parameters in the angle bracket, one is T, and the other is TOutput.

```C#
public static class BufferExtensions
{  
	public static IEnumerable<TOutput> AsEnumerableOf<T, TOutput>(this Buffer<T> buffer)
	{
		var converter = TypeDescriptor.GetConverter(typeof(T));
		foreach (var item in buffer)
		{
			TOutput result = (TOutput)converter.ConvertTo(item, typeof(TOutput));
			yield return result;
		}
	}
}
```

Basically, we are telling the method to get the information for T from the pass-in type parameter, and that overrides the behavior where C# compiler shares the T from buffer object to the method. So when we invoke the method, we have to pass in two types in the angle bracket, even though C# compiler knows the buffer has double for T, and so should the method.

```C#
var buffer = new Buffer<double>();
ProcessInput(buffer);
var asInts = buffer.AsEnumerableOf<double, int>();
foreach (var item in asInts)
{
	Console.WriteLine(item);
}
```

In the eyes of C# complier, a generic method has a method signature including a regular method signature plus the number of type parameter. The name of type parameter doesn't matter just like regular parameter name is not included in the regualr method signature.