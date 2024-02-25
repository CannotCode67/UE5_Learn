C# supports the Generics feature to parameterize the type. When we pass a parameter into a method, we usually need to specify the parameter type. With Generics, we can not only parameterize the value, but also the type. In a method scenario, this sounds like method overload, because when we are accepting different variable type, the underlying code must be different. Well, yes, the code is different, but with Generics, the difference is made automatically. This automation is simply getting the type when someone really passes a parameter, and replacing the type for the variables we specify. So, it is like method overload automation, but it only works when the true difference in code is just about the variable type. But Generics is more, it can be appied to classes, interfaces, etc. to help creating more concise and versatile code.

Generics allows code reuse with type safety. We no longer need to have a different version of code just because the variable types are different. Generics defers this type decision making until someone invokes the method or creates an object from a class, etc. So it is still strongly-typed. 

Think of T is a variable for type, and the syntax for a class using Generics is to use angle braket to wrap around a type variable after the class name. Now, wherever we use T instead of the actual type, there will be replacement for the type when someone uses the class. T is by convention the choice for type variable.

```C#
public class CircularBuffer<T>
{
	private T[] buffer;
	public T Read()
	{//... more code}
	// ... more code
}
```

When someone creates an object of Generics class, he needs to specify the type in angle bracket. And this specified type would then be used to replace all the T types inside the code.

```C#
var buffer = new CircularBuffer<double>(); 
```

