Surprisingly, delegate can work with generics as well. We have learnt delegates can parameterize method into a type. The delegate signature contains the return type and the take-in parameter number and their types. And if a delegate is a generic delegate, this generic variable can be used for return type and take-in parameter type.

Using generic delegate with generic class or interface, we can have T of the class or interface share across, so the T for delegate is the same as the T for class.

```C#
public delegate void Printer<T>(T data);
public static class BufferExtensions
{   
	public static void Dump<T>(this IBuffer<T> buffer, Printer<T> print)
	{
		foreach (var item in buffer)
		{
			print(item);
		}
	}
}
```

Now, when the buffer object is created, it has double for T, so Dump<>() should have double for T, and the delegate variable print should also have double for T. Because of that, the method ConsoleWrite() actually doesn't have the right type to fit in the print inside the Dump<>(). The following code would get an error when we put either object or double inside the new Printer angle bracket. To fix this, we change the ConsoleWrite() method's parameter type to double. But in reality, we might just not put the method into our delegate variable.

```C#
var buffer = new Buffer<double>();
ProcessInput(buffer);
var consoleOut = new Printer<>(ConsoleWrite);
buffer.Dump(consoleOut);
static void ConsoleWrite(object data)
{
	Console.WriteLine(data);
}
```

Delegate syntax allows us to define our own custom delegate, but .Net library actually provides built-in delegates, so we don't need to explicitly write one for ourselves. And guess what, those built-in delegates are generic deleagtes, so there are only a few of them, but it is enough to handle most situations. We would study the most common ones, Func, Action, and Predicate.

First, let's study Action<>. The Action delegate defines a delegate signature where the return type is void, and the take-in parameter number and type are defined by the type specified in angle bracket with maximum of parameter number up to 16. For example, Action<> can be used to replace the Printer<> delegate we define because Printer<> delegate has a return type of void.

```C#
public static class BufferExtensions
{   
	public static void Dump<T>(this IBuffer<T> buffer, Action<T> print)
	{
		foreach (var item in buffer)
		{
			print(item);
		}
	}
}
```

```C#
static void ConsoleWrite(double data)
{
	Console.WriteLine(data);
}
Action<double> print = ConsoleWrite;
var buffer = new Buffer<double>();
ProcessInput(buffer);
buffer.Dump(print);
```

Now, we have seen the built-in delegates help us write less code, before we study other built-in delegates, there is also a syntax called anonymous method, to help us write less code, and it achieves that goal by saving us for explicitly writing a method elsewhere.
In reality, often someone writes a delegate variable so other can add their methods into it, so usually the person who writes the delegate variable and the person who writes a method getting into that delegate variable are not the same person. However, when we work as both roles, with regular syntax, we need to explicitly define a method outside of the method where we declear our delegate variable, then come back and assign it to the delegate variable. The anonymous method syntax allows us to write the method implementation at the place where we assign it to the delegate variable.

The delegate keyword takes the place where there would have been the method name. Then the rest is just like writing a method definition and implementation.
```C#
Action<double> print = delegate(double data) 
{Console.WriteLine(data);}
```
A even shorter version is to use lambda expression in C#.
```C#
Action<double> print = d => Console.WriteLine(d);
```

This anonymous method syntax works quite well when the implementation is simple. Imaging a vey complex implementation cluttered in this line of code, we might not be able to resist the impulse to call on immediate refactoring.

Next built-in delegate we look at is Func<>. Func<> always has to return a value, so it cannot return void. How do we define that? The last type parameter we give it to Func<> would be its return type, so we always need to give at least one type parameter to Func<>. For example, to fit in `Func<double>` , a method would take no parameter but return a double. To fit in `Func<double, double>` , a method would take a double parameter and return a double.
```C#
Func<double, double> square = d => d * d;
Func<double, double, double> add = (x, y) => x + y;
```

The last built-in delegate we look at is `Predicate<T>`. Predicate<> always return a boolean. So we don't need to specify the return type like Action<>, we just put whatever take-in parameter type into the angle bracket.
```C#
Predicate<double> isLessThanTen = d => d < 10;
```

One more built-in delegate to cover, and that is `Converter<Tinput, Toutput>`. Converter delegate takes in two type parameters, first one for input parameter type, second one for return type, and they have to be different, that is why the name is converter.
```C#
Converter<double, string> convertert = d => d.ToString();
```

Before, we learnt delegate is extremely useful for raising event, because an event is just a multi-casting delegate with a few restrictions. We mentioned by convention, an delegate for event would have two special parameters, the sender of type object, and the argument of type EventArgs, and return type of void.
```C#
public delegate void GradeAddedDelegate(object sender, EventArgs args); 
```
Guess what, a built-in delegate is provided so we don't need to explicitly write a delegate for event everytime. And that built-in delegate also follows that convention. The EventHandler<> delegate defines a signature where the first parameter is always sender of type object, and the second one is an object carrying related information, and it needs to be type of EventArgs, or type derived from EventArgs. 
```c#
public event EventHandler<EventArgs> ItemDiscarded;
```
To understand this better, EventArgs is a base class written to work with event, as a information carrier. An object of type EventArgs carries no additional information, it is used when we don't need to pass any information. If we need to pass some information, we need to write a class that is derived from EventArgs, so the derived class would inherit all the codes for working with event. Inside this class, we set the field or property so an object of this class can carry related information.
```C#
public class ItemDiscardedEventArgs<T> : EventArgs
{
	public ItemDiscardedEventArgs(T discard, T newItem)
	{
		ItemDiscarded = discard;
		NewItem = newItem;
	}
	public T ItemDiscarded {get; set;}
	public T NewItem {get; set;}
}
```
Now we have a class to pass event information, we can put it into the EventHandler<>, but we can actually leave it as EventArgs, it would still work.
```C#
public event EventHandler<ItemDiscardedEventArgs<T>> ItemDiscarded;
```
When we raise the event, we are invoking the delegate variable by passing it the required parameters, so any method subscribed to this event/delegate variable can be invoked with parameters passed in.
```C#
public override void Write(T value)
{
	base.Write(value);
	if (thisQueue.Count > capacity)
	{
		var discard = thisQueue.Dequeue();
		OnItemDiscarded(discard, value);
	}
}
private void OnItemDiscard(T discard, T value)
{
	if (ItemDiscarded! = null)
	{
		var args = new ItemDiscardedEventArgs<T>(discard, value)
		ItemDiscarded(this, args);
	}
}
```
We can write a method that is suitable for this event, and implementation that processes the information passed in.
```C#
static void Main(string[] args)
{
	var buffer = new CircularBuffer<double>(capacity:3)
	buffer.ItemDiscarded += buffer_ItemDiscarded;
	//...more code
}
static void buffer_ItemDiscarded(object sender, ItemDiscardedEventArgs<double> e)
{
	Console.WriteLine($"Buffer full. Discarding{e.ItemDiscarded}, new item is {e.NewItem}");
}
```