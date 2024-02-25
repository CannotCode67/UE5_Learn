Generics interface is like a generics class. We need to wrap around the type variable with angle brackets and put it after the interface name. Then, any undecided types will be hold with type variable T, and later replaced with a specific one.

```C#
public interface IBuffer <T> 
	bool IsEmpty {get; }
	void Write(T value);
	T Read():
}
```

We implement interface like usual, now both the interface and the class use the same variable type for places where T are placed.

```C#
public class CircularBuffer<T> : IBuffer<T>
{
	// ...more code
}
```

Remember we learnt a interface defines a type just like a class. A method can choose to take in an object which class implements an interface, but the parameter typer can be the interface type, rather than the class type. With that, the develop who writes the ProcessInput() doesn't need to know what buffer underneath is, either the concrete implementation. He just needs to know the method provided by the interface, and invoke them as needed.

```c#
public class Buffer<T> : IBuffer <T>
{// ...more code}
var buffer = new Buffer<double>();
ProcessInput(buffer);
private static void ProcessInput(IBuffer<double> buffer)
{// ...more code}
```

All these encapsulation and indirection are not just for putting the OOP into action, but also providing actual benefits when there are multiple developers working on different part of the program. Of course, in real life, they still communicate and check on each other's codes, but it would be so much harder to develop a project separately without all these encapsulation and indirection technique.

When a generics class inherits a base generics class, the type variable (by convention is T), has to be the same. If one uses Z, then the other would need to change to Z as well. Becasue when a generics class gets constructed, the type is treated as a parameter. And the derived class would have the actual type stored in its type variable T, but if the base class has a type variable Z, then Z would never have a real type to fill in, because we are only receiving one real type, and that's for T, not Z. As a result, the base class cannot be constructed for the lacking a real type to fill in Z.

```C#
public class Buffer<T> // a type variable goes into angle bracket.
{}
public class CircularBuffer<T> : Buffer<T>
{} // Both angle brackets should have the same type variable like T
```

One very popular generics interface shipped with the .Net core library is the IEnumerable<>. T goes into the angle bracket. When we study the execution flow, we learn the foreach loop statement, and we are suggested to use it whenever possible for its conciseness and great readability. But that implicitly leaves a question, when it is not possible? We know foreach statement makes a couples of assumptions for the index variable, but there is one more requirement for a collection to be able to work with foreach statement. One of the answers to that requirement is the IEnumerable<> interface.

We can tell the base class to implement IBuffe<> interface, at the same time implement IEnumerable<> interface as well. But we can also tell the IBuffer<> interface to implement the IEnumerable<> interface. Yes, an interface can implement another interface. The syntax is similar to a class implementing an interface. But the rule seems a little bit different, we don't need to include the members defined in IEnumerable<> interface in the IBuffer interface. A class implementing IBuffer interface would need to have all those members, members defined in IEnumerable<> and members defined in IBuffer<>.

```C#
public interface IBuffer<T> : IEnumerable<T>
{// ...more code}
```

However, if we use the F12 trick to look inside IEnumerable<> interface, we will find out it also implements another interface called IEnumerable, without the generics. The IEnumerable interface only has one method, GetEnumerator(), and its return type is IEnumerator. The IEnumerable<> interface also has only one method, and its name is the same, GetEnumerator(), but this time and return type is IEnumerator<>.

```c#
public interface IEnumerable<out T> : IEnumerator
{
	IEnumerator<T> GetEnumerator();
}

public interface IEnumerable
{
	IEnumerator GetEnumerator();
}
```

Let's see how we can implement the GetEnumerator() method defined in IEnumerator<> interface. Because many collections shipped with the .Net core library implement IEnumerator<> interface, and if we use such a data structure to build upon, then we can just invoke the collection's own GetEnumerator() method.

```C#
public IEnumerator<T> GetEnumerator()
{
	return thisQueue.GetEnumerator();
}
```

If it is not, we can mannually write this method, and through this, we can do whatever we want to the value before giving it out. It seems we need to use yield return syntax, so the C# complier would build a state machine to implement this IEnumerator<>. A simple return would just return the first item and jump out of the loop. And IEnumerator<> type seems to be a collection class by itself, and it needs all the items to make the whole thing numerable. The keyword yield would be explained further when we study LINQ.

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

Next, we need to write the GetEnumerator() method defined in IEnumerable interface. But before we do that, we need to solve the problem where we have two GetEnumerator() methods in a class. The C# compiler would see them as the same method because both methods have the same method signature, and return type is not part of the signature. So it would not compile. The trick is to use explicit interface implementation, in which the method name includes the interface name and a dot. Now they are different methods for having different method names. Someone can invoke this method if he has the object with the type of the IEnumerable interface, or he needs to cast the object into the type of IEnumerable interface. The implementation is calling the GetEnumerator() method we implement for IEnumerable<> interface. We need more research to understand this.

```C#
public IEnumerator IEnumerable.GetEnumerator()
{
	return this.GetEnumerator();
}
```

Another two common interfaces shipped with .Net are the IEqualityComparer<> interface, and a slighlty modified version, IComparer<> interface. IEqualityComparer<> inreface is used to tell if two things are equal, and IComparer<> interface goes one step further by telling if one thing is more or less than the other.

We often see Set collections to use these two interface because set collections don't accept duplicated element, and they need a way to tell if two elements are the same to prevent a duplicated element. For primitive types, usually the default behavior of the set collections can tell the difference, but if it is complex like a custom class, they would need the help from those two interfaces.

The constructor of the HashSet is overloaded, so it can take in an object which is from a class implementing the IEqualityComparer<>, and use the implemented members in that object to differentiate two things. Well, since all we need are the implemented members from the interface, this whole class usually just contains them and nothing else.

IEqualityComparer<> interface has two members. Equals() method and GetHashCode() method.
For the first method, we need to provide the logic for differentiating two things. For the second one, we tell the HashSet based on what to generate its hash code. In this example, we know very well employee is the type we are working with, so there is no need for the class to take a generics parameter.

```C#
public class EmployeeComparer : IEqualityComparer<Empolyee>
{
	public bool Equals(Employee x, Employee y)
	{
		return String.Equals(x.Name, y.Name);
	}
	public int GetHashCode(Employee obj)
	{
		return obj.Name.GetHashCode();
	}
}
```

```C#
var newSet = new HashSet<Empolyee>(new EmployeeComparer());
```

Now, if we are using SoredSet for its sorting behavior, then we need to provide an object from a class implementing the IComparer<> interface. The IComparer<> interface enforces one method, the Compare() method. The way the collection treats the outcome of this method, would be if it is less than zero, then the first thing is less than the second thing, and vice versa.

```C#
public class EmployeeComparer : IEqualityComparer<Employee>
                              : IComparer<Employee>
{                             
	public bool Equals(Employee x, Employee y)
	{
		return String.Equals(x.Name, y.Name);
	}
	public int GetHashCode(Employee obj)
	{
		return obj.Name.GetHashCode();
	}
	public int Compare(Employee x, Employee y)
	{
		return String.Compare(x.Name, y.Name);
	}
}
```

One tip to improve codes using generics is to wrap those codes inside a custom class, to make the main code eaiser to understand and read.

Other common generic interfaces to learn are these.
![[capture-01.JPG]]




