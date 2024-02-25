Generics are great for working with collections because codes in collections rarely cares about the type of elements to store. However, sometimes we want to interact with the element, and a generic type is only guaranteed it is a type of System.Object, and if the members provided in System.Object cannot satisfy our need, we are out of luck. That is why we have generic constraints. A generic constraint can force a type parameter to have certain characteristics.

To apply one or more generic constraints, we use the keyword `where` with the following syntax. There are a couple different types of constraints and we will study them in detail.
```C#
IBuffer<T> where T: class, Item, new()
{}
```

First type of generic constraint is the constraint requiring the T to be a reference type or value type. If it is a reference type, we use class, and struct for value type. This constraint would always be the first constraint in the syntax if we have one.
Forcing T to be a class makes it possible to be null, as value type cannot be null. Without the class constraint, we need to use the syntax in the comment, default() method would return null if T is a class, return 0 if T is a numerical value types.
```C#
public class SqlRepository<T> : IRepository<T> where T: class, IEntity
{
	//...more code
	public T FindById(int id)
	{
		T entity = null;
		//T entity = default(T);
		return entity;
	}
	//...more code
}

```
Sometimes, we not just want T to be a class or struct, we want it to be a certain class, we can use the class name instead of the word class. Person is the class in our example, and T can be Person or any class derived from Person.
```c#
public class SqlRepository<T> : IRepository<T> where T: Person, IEntity, IDisposable
{}
```
And we can even have another type parameter and put that into the constraint. In our example, it is basically saying T has to be T2 or something derived from T2.
```c#
public class SqlRepository<T, T2> : IRepository<T> where T: T2, IEntity, IDisposable
{}
```
In cases where we put a certain class into the constraint, we usually put a base class there, so T would have some room to vary. If we put a specific concrete class there, then it defeats the purpose of using T in the first place.

Second type of generic constraint is the constraint requireing the T to implement a certain interface. This is also the constraint for T to have certian methods or fields or properties, because we can define all that in interface and ask the class to implement them. Generic constraint of interface comes second after the generic constraint of class/struct. And if there are multiple generic constraints of multiple interfaces, we use comma to separate them.
```c#
public class SqlRepository<T> : IRepository<T> where T: class, IEntity, IDisposable
{}
```

The third type of generic constraint is the constraint specifying whether an object of T can be created through the keyword new. This characteristics cannot be specified using the constriants before. And this type of constraint has to the last one in syntax if there is one.
```c#
public class SqlRepository<T> : IRepository<T> where T: class, IEntity, new()
{
	//...more code
	public T FindById(int id)
	{
		T entity = new T();
		return entity;
	}
	//...more code
}
```

If there are multiple type parameters needed to have constraints, we can use multiple keyword `where` to specify their constraints separately like this.
```c#
public class SqlRepository<T, T2> : IRepository<T> where T : class, IEntity
                                                   where T2 : class, IDisposable
{}
```

At last, generic constraints can be used in interface, because the constraint is really describing T, not the interface.
```c#
public interface IRepository<T> : IDisposable where T : class, IEntiy
{}
```
This is syntactically legal piece of code. However, conceptually, interface is about abstraction, which is not the direction we move towards with generic constraints. The more generic constraints we have, the closer we get to a concrete implementation. Therefore, it would make sense to apply generic constraints in class where some real implementations occurs.