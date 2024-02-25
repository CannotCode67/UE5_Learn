We now study a concrete example where reflection is heavily used. That is the Ioc container. What is it? Ioc stands for inversion of control, which is a concept stating objects do not create other objects on which they rely to do their work, and instead they get those objects from an outside source. If object A needs to invoke a method in object B, object A should not have the ability of creating object B, instead, it obtains an object B from an outside source through a request or whatever. Another related concept is dependency injection. It generally means passing a dependent object as a parameter to a method, rather than having the method create the dependent object.

Now back to the container. So we have a ILogger<> interface, and different loggers implement this interface, some can write log to a Sql server, others to XML file on disk, etc. Then, we have a class related to a certain business service and this service would need a logger writing information to a Sql server. Now, instead of creating this logger object inside the class, we send a request to the container asking for something implementing ILogger<> interface. And the container checks our class using reflection, and return a suitable logger, which logs to Sql server. That's the whole story about our Ioc container example. Time to translate this into code.

We want a container objected created, and it can accept configuration telling it how to handle request. Then, when it takes a request, it returns a proper logger.
```C#
[TestMethod]
public void Can_Resolve_Types()
{
	var ioc = new Container(); //container object created before configuration
	ioc.For<ILogger>().Use<SqlServerLogger>(); //setting configuration

	var logger = ioc.Resolve<ILogger>(); //passing in request

	Assert.AreEqual(typeof(SqlServerLogger), logger.GetType()); //should be equal
}
```
Let's write the Container class, because we can see from above we are calling methods after methods on the container object. The way we invoke `Use<T>()` method forces the `For<T>()` method to return something that contains the `Use<T>()` method, can be the container itself or something else. As return, our API for configuration is fluent to read and use.

Because we aim to achieve the API in a fluent style, it would make more sense the `For<T>()` returns
something otherthan the container itself, but using container is also doable, it is just using a nested class `ContainerBuilder` would make this much cleaner in logic. If we choose to return the container, a better way to go would be to write one single method that takes both parameters to handle the configuration, resulting a regular style for API.

Another very common feature is we offer two versions for each method, one that takes generic parameter, and the other takes System.Type parameter. This is not just for ease of usage. We know C# at low level combines the unbound generic type and the generic type parameter to form closed constructed type, and unbound generic type doesn't work with angle bracket syntax.

The logic in the container class is simple, we internally use a dictionary for mapping request type to logger types. The `For<T>()` method returns a `ContainerBuilder` object, and the `Use<T>()` method stores the two parameters inside the dictionary. The `Resolve<T>()` method tries to get a value from the dictionary using the parameter as the key. If it does find a value, it uses Activator class to create an instance of the type equaling to the value and returns it. If not, it throws an exception.
```C#
public class Container
{
	Dictionary<Type, Type> _map = new Dictionary<Type, Type>();
	public ContainerBuilder For<TSource>()
	{return For(typeof(TSource));}
	public ContainerBuilder For(Type sourceType)
	{
		return new ContainerBuilder(this, sourceType);
	}
	public TSource Resolve<TSource>()
	{
		return (TSource)Resolve(typeof(TSource));
	}
	public object Resolve(Type sourceType)
	{
		if (_map.ContainsKey(sourceType))
		{
			var destinationType = _map[sourceType];
			returtn Activator.CreateInstance(destinationType);
		}
		else
		{
			throw new InvalidOperationException("Could not resolve " + 
                                                sourceType.FullName);
		}
	}
	public class ContainerBuilder
	{
		Container _container;
		Type _sourceType;
		public ContainerBuilder(Container container, Type sourceType)
		{
			_container = container;
			_sourceType = sourceType;
		}
		public ContainerBuilder Use<TDestination>()
		{return Use(typeof(TDestination));}
		public ContainerBuilder Use(Type destinationType)
		{
			_container._map.Add(_sourceType, destinationType);
			return this;
		}
	}
}
```

We have successfully created an Ioc container that can accept configuration, and handle request based on configuration, but it is very fragile. Let's say we have an interface `IRepository<T>`, and we have a class `SqlRepository<T>` that not only implements the interface, but also requires an ILogger object in its constructor.
```C#
public interface ILogger
{}
public interface IRepository<T>
{}
public class SqlRepository<T> : IRepository<T>
{
	public SqlRepository(ILogger logger)
	{}
}
```
We can tell the container for `IRepository<Employee>`, we should use `SqlRepository<Employee>` for return. The configuration is no problem, we just add key-value pair into the dictionary. However, when we ask the container to resolve, we get an exception.
```C#
[TestMethod]
public void Can_Resolve_Types_Without_Default_Ctor()
{
	var ioc = new Container();
	ioc.For<ILogger>().Use<SqlServerLogger>();
	ioc.For<IRepository<Employee>>().Use<SqlRepository<Employee>>();
	
	var repository = ioc.Resolve<IRepository<Employee>>();

	Assert.AreEqual(typeof(SqlRepository<Employee>), repository.GetType());
}
```
The reason is when we pass only the type into the parameter of CreateInstance() method, we are invoking a version of this method that only calls the type's parameterless constructor. And the class `SqlRepository<T>` only has one custom constructor that takes in an `ILogger` object as parameter. As you may recall, once we write our custom constructor, the default one would no longer be implicitly provided. Therefore, the CreateInstance() fails to invoke a constructor that doesn't exist, makes perfect sense.

Now, the good thing is CreateInstance() method is overloaded, and one of its version takes in the type, and a constructor parameters array. The bad thing is the container class has no idea what parameters are required by the constructor. Once again, we can use reflection to solve the problem. Before invoking CreateInstance() method, we need to add some logics to dig out the parameters information through reflection. It would be better that we extract this process as one single method `CreateInstanceWithParameters()`, and replace the CreateInstance() method with it in Resolve() method.
```C#
public object Resolve(Type sourceType)
	{
		if (_map.ContainsKey(sourceType))
		{
			var destinationType = _map[sourceType];
			returtn CreateInstanceWithParameters(destinationType);
		}
		else
		{
			throw new InvalidOperationException("Could not resolve " + 
                                                sourceType.FullName);
		}
	}
private object CreateInstanceWithParameters(Type destinationType)
{
	var q = destinationType.GetConstructors()
							.OrderByDescending(c => c.GetParameters().Count())
							.First()
							.GetParameters()
							.Select(p => Resolve(p.ParameterType))
							.ToArray();
	return Activator.CreateInstance(destinationType);
}
```
We take a look of the logics in that huge LINQ statement. First, GetConstructors() would return all the constructors metadata the type has. OrderByDescending() with that lambda express inside is basically saying order the constructors metadata by descending order using parameter number as criteria, so the constructor with the most parameters would come on top. First() would return the first element, so the constructor metadata with the most parameters gets returned. But it might throw an exception if the type really has no constructor at all. GetParameters() would return all the parameters metadata this constructor metadata contains. Then, Select() with that lambda express is saying extract the parameter type from each parameter metadata and put it into the Resolve() to instantiate an object, then select all the instantiated objects. Only the types which container has configuration with, would be instantiated. ToArray() means put the selection into an array, because the CreateInstance() method accepts parameters in an array.

Now we can create a `SqlRepository<Employee>` object through container's Resolve() method. But we still have a few problems with our container. The logics in big LINQ statement has no exception handling code, so if an exception is thrown, our program crashes. Next, the Resolve() method can creatre the objects we need for constructor parameters because we know `SqlRepository<T>` needs an ILogger object, we know `ILogger` is a key inside the configuration dictionary, and we know the return value is a SqlServerLogger object implementing the ILogger interface. Of course, you might argue that ILogger is an interface, so an object of ILogger must be something of a class which implements the interface, and it makes no sense to pass something without the interface when it is the request. Therefore, the last assumption should be true. But the first one and the second one are not guaranteed. Maybe we just don't have that configuration to meet the requirement, and in those cases, we throw an exception. We can mitigate that situation with some additional logic in Resolve() method. If the type is not an abstract type, meaning it can be instantiated directly. We just instantiate an object with that type even though we have no configuration for it. Mitigate but not eradicate, because if that concrete type's constructor happens to require something, and that something is abstract and not in our configuration, then our program would still crash. 
```C#
public object Resolve(Type sourceType)
	{
		if (_map.ContainsKey(sourceType))
		{
			var destinationType = _map[sourceType];
			returtn CreateInstanceWithParameters(destinationType);
		}
		else if (!sourceType.IsAbstract)
		{
			return CreateInstanceWithParameters(sourceType);
		}
		else
		{
			throw new InvalidOperationException("Could not resolve " + 
                                                sourceType.FullName);
		}
	}
```
Think of it this way, if we don't have the configuration, then container doesn't know how to handle request, which is acceptable. We cannot expect the container to know how to handle something we haven't taught it about. But it would be nice if the container know how to deal with generic type because for now, we set the configuration so that the container knows when the request is `IRepository<Employee>`, it should return an object of `SqlRepository<Employee>`. But if the request is `IRepository<Customer>`, it would have no idea how to handle it. Basically, we want the container to use `SqlRepository<T>` when the request is `IRepository<T>`.

We would like to achieve that, at the same time, maintain the fluent API style. However, the unbound generic type `IRepository<>` doesn't work with the angle bracket syntax. The only place it can go is the parameter parenthesis of typeof() method. Now you know why we go thourgh all those troubles setting up two versions of each method.
```C#
public class InvoiceService
{
	public InvoiceService(IRepository<Customer> repository, ILogger logger)
	{}
}
[TestMethod]
public void Can_Resolve_Concrete_Type()
{
	var ioc = new Container();
	ioc.For<ILogger>().Use<SqlServerLogger>();
	ioc.For(typeof(IRepository<>)).Use(typeof(SqlRepository));
	
	var service = ioc.Resolve<InvoiceService>();

	Assert.IsNotNull(service);
}
```
But now, this would throw an exception, when the container tries to resolve a specific IRepository<> like `IRepository<Employee>`. Because the dictionary holds the key value of `IRepository<>`, the unbound generic type, it is not gonna match any specific type. What do we do then? We can yet add another piece of logic into the Resolve() method. When someone sends a request of a specific generic type like `IRepository<Employee>`, the container can look into this metadata and extra the unbound generic part of it to check for matching. If the container finds a match, it can go building the correct `SqlRepository<T>` type using the type parameter contained in that passed in metadata.
```C#
public object Resolve(Type sourceType)
	{
		if (_map.ContainsKey(sourceType))
		{
			var destinationType = _map[sourceType];
			returtn CreateInstanceWithParameters(destinationType);
		}
		else if (sourceType.IsGenericType && 
                 _map.ContainsKey(sourceType.GetGenericTypeDefinition()))
        {
	        var destination = _map[sourceType.GetGenericTypeDefinition()];
	        var closedDestination = 
                            destination.MakeGenericType(sourceType.GenericTypeArguments)
            return CreateInstanceWithParameters(closedDestination);
        }
		else if (!sourceType.IsAbstract)
		{
			return CreateInstanceWithParameters(sourceType);
		}
		else
		{
			throw new InvalidOperationException("Could not resolve " + 
                                                sourceType.FullName);
		}
	}
```
The GetGenericTypeDefinition() method returns the unbound generic type metadata. As you may recall, when we have the unbound generic type, we need to make it a closed constructed type using MakeGenericType() method. We pass the generic type parameter of the source type into that method. GenericTypeArguments property returns all type parameters. And with all that, our container can accept unbound generic type as configuration and resolve a related specific one upon request.
Our example of Ioc container ends here, but it is still not robust enough for production, not even close. The merit of this is we learn reflection and how it interacts a little bit differently when generic is involved. For production, there are many fully featured, well tested, powerful Ioc containers provided by many parties, go use those.