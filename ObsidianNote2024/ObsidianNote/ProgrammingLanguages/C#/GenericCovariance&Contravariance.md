Although the title is not specified, the covariance and contravariance features only work with interfaces and delegates that are using generics. To understand the concept better, we need to go back and revisit one of those unanswered questions.

Remember the very useful generic interface called `IEnumerable<T>`, its definition is following.
```C#
public interface IEnumerable<out T> : IEnumerator
{
	IEnumerator<T> GetEnumerator();
}
```
The last time we study `IEnumerable<T>`, we neglect the keyword out in the angle bracket. This time, it is the subject of our study because the keywrod out is known as a genric modifier, which makes the type parameter covariant. By the way, those without modifiers are called invariant generic type parameters. We don't need to put out in front of all the Ts, we just need to put it in the interface name, then all the Ts in the curly braces are covariant. And now we also have a constraint where we can only have T as return type. If we have a method takes in T as parameter, we would have an error.
```C#
public interface IEnumerable<out T> : IEnumerator
{
	IEnumerator<T> GetEnumerator();
	void SomeMethod(T something); //this lind of code is not allowed for covariant T
}
```

What does that allow us to do exactly? Well, it allows us to treat this T as its base class. First, we have a class called Employee derived from a class called Person.
```C#
public class Employee : Person
{}
```
Then, we have a generic class called `Repository<T>`, and this class implements the `IEnumerable<T>` interface. Its method FindAll() returns `IEnumerable<T>`. The object empolyeeRepository has a type of Employee for T, so FindAll() should return `IEnumerable<Employee>`. We can however, use a variable of type `IEnumerable<Person>` to store the return value, treating `IEnumerable<Employee>` as `IEnumerable<Person>`. That's because T in `IEnumerable<T>` is covariant.
```C#
IEnumerable<Person> temp = empolyeeRepository.FindAll();
```
From an object-oriented point of view, this is how things are supposed to be. But if T is an invariant  generic type parameter, we would have to treat `IEnumerable<Employee>` as it is, even though it makes less sense from this point of view. 

But let's see things in a different POV. Let's say an interface has one method that allows user to read data from whatever the source is, and another method that allows user to write data into whatever the source is.
```C#
public interface IRepository<T> : IDisposable
{
	T FindById(int id);
	void Add(T newEntity);
	void Delete(T entity);
}
```
When we read from the source, we get Employee, and we are guaranteed Employee is a Person because Employee inherits from Person. So we are safe to treat Employee as Person. However, this doesn't work the other way. We invoke the Add() method to add a Person into the source, but a Person is not an Employee. Now you should see the problem if we put an out to modify T in `IRepository<T>`. Now, what do we do if we really need the covariant behaviour for our FindById() method? Well, in that case, we need to put it into another interface and tell `IRepository<T>` to implement it.
```C#
public interface IReadOnlyRepository<out T> : IDisposable
{
	T FindById(int id);
}
public interface IRepository<T> : IReadOnlyRepository<T>, IDisposable
{
	void Add(T newEntity);
	void Delete(T entity);
}
```
And once we have this setup, we can only invoke the FindById() method through `IReadOnlyRepository<T>` not `IRepository<T>`.
```C#
private static void GetPersonById(IReadOnlyRepository<Person> employeeRepository, int id)
{
	Console.WriteLine(employeeRepository.FindById(id));
}
```

Suppose we now have a class called Manager, and it is derived from Employee.
```C#
public class Manager : Employee
{}
```
When I have an Manager object, and I want to add it to the employeeRepository through the Add() method. The method would work only when we treat the employeeRepository as the type of `IRepository<Employee>`. Add() method would expect an Employee object, and Manager is Employee, so it automatically works. However, if I want to treat the employeeRepository as the type of `IRepository<Manager>`, I would get an error.
```C#
//This would work
private static void AddManagers(IRepository<Employee> employeeRepository)
{
	employeeRepository.Add(new Manager { Name = "Alex"});
}
//This would not work
private static void AddManagers(IRepository<Manager> employeeRepository)
{
	employeeRepository.Add(new Manager { Name = "Alex"});
}
```
I know things can be very confusing here. Because if T is invariant, then we shouldn't be able to add the Manager object through Add() method becasue Add() method expects an Employee object 
and Employee object only for the T is invariant. That should only be achievable when we use generic modifier `in` to make T contravariant. But it seems to work in tutorial, so maybe doable in older version of C#?
```C#
public IWriteOnlyRepository<in T> : IDisposable
{
	void Add(T newEntity);
	void Delete(T entity);
}
```
Now, treating employeeRepository as type `IRepository<Manager>` would not work unless the T is contravariant. You might say covariance works perfectly as that flows with the object-oriented sense, but contravariant? Treating a bunch of Employee objects as a bunch of Manager objects? That seems to be so wrong, and you are right, but that would never happen for contravariant T.

You might notice the keyword is `in` for contravariance, and `out` for covariance, that's because `in` means input, and that `in T` can only be used as input parameter. On contrast, `out T` can be used only for return type. With all that said, a method that takes in an Employee object must be able to take in a Manager object instead since a Manager object is an Employee object, but the situation where we get an Employee object and treat it as a Manager object would not happen for `in T`, because `in` T can only be used as input parameter. Getting an Employee object and treating it as a Person object is allowed by `out T`. But we cannot ask a method that takes in an Employee object to take in a Person object, because first of all, `out T` can only be used as return type, and secondly a Person object is not an Employee object, it just makes no sense.

Now, we understand contravariance and covariance better. We still have a problem. That's because if we make `IRepository<in T>`, we would get an error. Because we are not allowed to have a contravariant generic interface that implements a covarianct generic interface, which effectively makes the T in `IReadOnlyRepository<>` interface both covariant and contravariant. So what do we do if we want the Add() to have a contravariant T while keeping the `IReadOnlyRepository<out T>`?
We can create yet another interface to contain the Add() method, make this one contravariant, and tell the invariant `IRepository<T>` interface to implement both `IReadOnlyRepository<out T>` and `IWriteOnlyRepository<in T>`.
```C#
public IWriteOnlyRepository<in T> : IDisposable
{
	void Add(T newEntity);
	void Delete(T entity);
}
public interface IReadOnlyRepository<out T> : IDisposable
{
	T FindById(int id);
}
public interface IRepository<T> : IReadOnlyRepository<T>, 
                                  IWriteOnlyRepository<T>,
                                  IDisposable
{}
```
And in the main(), we need to call the Add() method through `IWriteOnlyRepository<in T>`.
```C#
private static void AddManagers(IWriteOnlyRepository<Manager> employeeRepository)
{
	employeeRepository.Add(new Manager { Name = "Alex"});
}
```
