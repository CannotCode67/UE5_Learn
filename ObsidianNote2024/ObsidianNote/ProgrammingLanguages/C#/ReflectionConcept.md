So far, we have work on code that invokes methods through a class, or an object, or a value, then we met generics. Now we know type can a parameter, but we merely scratch the surface with only generics. Reflection can take us further, and the idea of reflection is about using a type like an object because a type itself is a kind of data known as metadata.

Let's say we create a List of Employee. First, we have the Employee class.
```c#
public class Employee
{
	public string Name {get; set;}
}
```
Then, we would be able to create a List for it in Main().
```C#
static void Main(string[] args)
{
	var employeeList = new List<Employee>();
}
```
There is another way to do this. That's through reflection. We use the built-in Activator class because it has a static method called CreateInstance() that takes in a type, and return an object of that type. This functionality is very similar to the keyword new(), except CreateInstance() method by default invokes the constructor that takes no parameter. If there isn't one, the CreateInstance() method would throw an exception.
```C#
static void Main(string[] args)
{
	var employeeList = Activator.CreateInstance(typeof(List<Employee>));
}
```
We check for the object's type using GetType() method. This method would return a type, and we can do whatever we want with this piece of metadata.
```C#
static void Main(string[] args)
{
	var employeeList = Activator.CreateInstance(typeof(List<Employee>));
	Console.WriteLine(employeeList.GetType().Name);
}
```
The result of this would be List backtick 1, because that's saying it is List taking in 1 generic type. If we use a `dictionary<T1, T2>` here, we get a Dictionary backtick 2. The result clearly misses the Employee type information. Where do we get this information?
We get this from the metadata's GenericTypeArguments, and we are not sure if this is a field or property. Notice this would return a collection of generic type arguments, because there can be multiple ones. We loop through each item and write out the name.
```C#
static void Main(string[] args)
{
	var employeeList = Activator.CreateInstance(typeof(List<Employee>));
	Console.WriteLine(employeeList.GetType().Name);
	var genericArguments = employeeList.GetType().GenericTypeArguments;
	foreach (var arg in genericArguments)
	{
		Console.Write($"{arg.Name}")
	}
	Console.WriteLine();
}
```
The result now is List backtick 1 [Employee].  So the metadata does contain this information, but as we can see, C# treats `List<Employee>` not as a single type, but as the combination of a generic type and a generic type argument. This kind of combination is known as Closed Constructed Type which can be instantiated.

The CreateInstance() method takes in one type, if we want some flexibility where the collection type and item type can be defined separately, we need to create a method. Here we really can see the typeof() method is returning the metadata or type related to the given class name. Backthen, when we study class, we say when we define a class we define a type, which might cause confusion that class is type. But here, we really can see class and type are two separate things, and this is the key to understand reflection. In reflection, we do things with type, the metadata.
```C#
static void Main(string[] args)
{
	var employeeList = CreateCollection(typeof(List<>), typeof(Employee));
	Console.WriteLine(employeeList.GetType().Name);
	var genericArguments = employeeList.GetType().GenericTypeArguments;
	foreach (var arg in genericArguments)
	{
		Console.Write($"{arg.Name}")
	}
	Console.WriteLine();
}
private static object CreateCollection(Type collectionType, Type itemType)
{
	var closedType = collectionType.MakeGenericType(itemType);
	return Activator.CreateInstance(closedType);
}
```
Now that you understand reflection better, you may ask what it is for? Because from what we see here, all those codes just to replace a keyword new()? Doesn't the regular way make more senses?
Well, yes, if what you do is fairly simple, then don't bother do it in reflection way. However, reflection allows you to access private data, which is not possible through regular way. Also, imagine you can write a logic to invoke different methods based on situation during runtime, you may say an if statement would do it. Well, in a scenario where you don't have a clue of the given class, you would have to use reflection to look inside the class and make decisions during runtime.

Now, we see how we can invoke methods or access properties through reflection. First, we add a method to our Employee class, so we would have something to invoke.
```C#
public class Employee
{
	public string Name {get; set;}
	public void Speak<T>()
	{
		Console.WriteLine(typeof(T).Name);
	}
}
```
Next, we invokes it through reflection. The GetMethod() takes in the method name in string format, and returns a MethodInfo object with metadata. Then we can invoke the method through this object. Because this method is instance method, we need to give it an instance, and if no parameters are needed, we give it `null`. This would work if the method is not generic. But as we can see, `Speak<T>()` is a generic method.
```C#
static void Main(string[] args)
{
	var employee = new Employee();
	var employeeType = typeof(Employee);
	var methodInfo = employeeType.GetMethod("Speak");
	methodInfo.Invoke(employee, null);
}
```
Just like we can only instantiate an generic type with the generic parameter, a closed constructed type, we can invoke a generic method only when it is completed with a generic parameter. Hope you see the process here, under the hood, C# first turn a generic type into a regular type, then it treat it like a regular type. For generic methods, it is the same, we need to turn it into a regular method first before invoking it. Just like type having the MakeGenericType() method, we have MakeGenericMethod() for method.
```C#
static void Main(string[] args)
{
	var employee = new Employee();
	var employeeType = typeof(Employee);
	var methodInfo = employeeType.GetMethod("Speak");
	methodInfo = methodInfo.MakeGenericMethod(typeof(DateTime));
	methodInfo.Invoke(employee, null);
}
```
