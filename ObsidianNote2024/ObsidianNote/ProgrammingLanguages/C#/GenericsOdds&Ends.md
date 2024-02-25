This chapter contains pieces of information about generics, but they don't fit into other chapters and themselves alone are not really enough to be a chapter, so things you might overlook, or special cases that don't follow the standard rule, etc.

First we take a look ar enums. Enum is the short for enumeration, and it represents a set of related named integral constants. A named value usually has great readability. And once a variable is decleared as enum type, it's values can only be one of the constants in the enum.
```C#
public enum GameState {Running, Paused, Ended};
public class EnumExample : MonoBehaviour
{
	private GameState gameState;
	void Start()
	{
		gameState = GameState.Running;
	}
}
```

The pitfall for working enum with generics is  the generic constraint doesn't work with the enum. There is no way we can enforce the T to be an enum, becasue the way C# is designed. So the best we can do is to tell T to be struct, because enum are value type. Then use validating code to check for enum during runtime.
```C#
public enum Steps
{
	Step1,
	Step2,
	Step3
}
public static class StringExtensions
{
	public static T ParseEnum<T>(this string value) where T : System.Enum //error
	{
		return (T)Enum.Parse(typeof(T), value);
	}
}
class Program
{
	static void Main(string[] args)
	{
		var input = 'Step1';
		var value = input.ParseEnum<Steps>();
		Console.WriteLine(value);
	}
}
```


Second area where generic doesn't quite work is math problem. Here we have an example where we have a method to compute the sample average of a collection of numbers in double format. If we want the method to handle numbers in int format, we might think we can use generic for the method. But we would get an error because for T to be able to work with those numeric operator, it has to be numeric types. Unfortunately, there is no generic constraint to force T to be numeric types, or to be able to work with numeric operators. Maybe we can use IComparable<> interface to achieve the same functionality of >, <, = operators, but other operators are just no go. Therefore, for math operations, overloading method is probably the best we can do.
```C#
class Program
{
	static void Main(stringp[] args)
	{
		var numbers = new double[] {1, 2, 3, 4, 5, 6};
		var result = SampledAverage(numbers);
		Console.WriteLine(result);
	}
	private static T SampledAverage<T>(T[] numbers) // eror
	{
		var count = 0;
		var sum = 0;
		for (int i = 0; i < numbers.Length; i+=2)
		{
			sum += numbers[i];
			count += 1;
		}
		return sum / count;
	}
}
```


Generics are strong typed, and that is a good thing for most of the time. But sometimes we find that is a little bit too harsh. Let's say we have a list to store an item in int format. We are perfectly capable of adding a item of int to the collection, but not for item of double. Item in int and item in double are probably both numeric types, and we might need to keep both due to the fact people who contribute to this values are not very strict. The way we can workaround is to have a base class for the generic one to inherit from, so the list can choose to accept the type of base class, allowing the operation of adding both item of int and item of double into the list.
```C#
class Program
{
	static void Main(string[] args)
	{
		var list = new List<Item>();
		list.Add(new Item<int>());
		list.Add(new Item<double>());
	}
}
public class Item<T> : Item
{}
public class Item
{}
```


The last area we want to touch on is static member with generic. Let's say we have a generic class Item<>, and it has a static integer field called InstanceCount, whenever an object is created, the static field plus one. Now we instantiate two items of int, and one item of string, guess what the InstanceCount would be. Well, the result is 2, and 1. The reason would be `Item<int>` is a class, and `item<string>` is effectively a different class. Although they belong the class `Item<T>`, but the static member follows the closed constructed type, not the unbound generic type. If we want `Item<int>` and `Item<string>` to share the same static field, we can create a base class and move the static field and its related behavior code into the base class. Now, the result would be 3, and 3. But even thoug we write `Item<int>.InstanceCount` and `Item<string>.InstanceCount`, we are actually accessing the `Item.InstanceCount`. This is a little bit weird, but that's how C# is designed, and we need to be aware of that.
```C#
class Program
{
	static void Main(string[] args)
	{
		var a = new Item<int>();
		var b = new Item<int>();
		var c = new Item<string>();

		Console.WriteLine(Item<int>.InstanceCount);
		Console.WriteLine(Item<string>.InstanceCount);
	}
}
public class Item<T> : Item
{
	//public Item()
	//{
		//InstanceCount += 1;
	//}
	//public static int InstanceCount;
}
public class Item
{
	public Item()
	{
		InstanceCount += 1;
	}
	public static int InstanceCount;
}
```

![[capture-02.JPG]]
