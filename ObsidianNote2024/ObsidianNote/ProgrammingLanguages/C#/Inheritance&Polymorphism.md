Inheritance is one of the pillars of object-oriented programming, the other two are encapsulation and polymorphism. In C#, inheritance describes a relationship between two classes. If class A inherits from class B, then class A would automatically have the class members of class B, and class B is called the base class of class A. That sounds very useful, let's see how we can establish such relationship.

When a class inherits another class, we use a colon and the inherited class name, like the example. Now the Book class would have the Name property so that the constructor can assign the passed name parameter to Name.

>public class NamedObject
>{
>	public string Name
>	{get; set;}
>}
>public class Book : NamedObject
>{
>	public double[] grades;
>	public Book(string name)
>	{
>		grades = new double[10];
>		Name = name;
>	}
>}

For the outside, we can access the property using book object just like we access grades field of book object.

>var book = new Book("NewBook");
>book.grades[0] = 12.7;
>book.Name = "NewerBook";

In the example above, the NamedObject class doesn't have an explicit constructor. But what if it does, and the constructor requires some parameters. In that case, we would need to explicitly invoke the base class's constructor when we write the Book class constructor. Again, using a colon but this time, we use the keyword base to refer to the base class. The keyword base is an implicit variable referring to the base class just like the keyword this.

>public class NamedObject
>{
>	public NamedObject(string name)
>	{Name = name};
>	public string Name
>	{get; set;}
>}
>public class Book : NamedObject
>{
>	public double[] grades;
>	public Book(string name) : base(name)
>	{
>		grades = new double[10];
>		Name = name;
>	}
>}

There is always something implicit about the C# language. We mention yet another implicit variable base, now all classes in C# implictly inherits from a very base class, the System.Object class. But in C#, a class can only inherits directly from one base class, so giving two class names after the colon would not complie.

>public class Book : NamedObject, Object
>{}

Instead, there is a chain of inheritance, although there is no need to explicitly write out the inheritance of System.Object class.

>public class NamedObject : Object
>{}
>public class Book : NamedObject
>{}

Therefore, all classes have those methods like ToString(), ReferenceEquals(), etc. And if we get a variable of type object, which is a bad idea, then this variable can basically store everything, but it is not type safe, meaning before I use this variable, I have no idea what type of value is stored inside, then if I don't validate it first, it can crash my program easily.

Now, when we use inheritance and mix it with polymorphism, we often come across abstract class. The abstract class cannot be instantiated directly, and must have at least one abstract method. Otherthan that, an abstract class is no different from a regular class, it can have field, property, and methods that are implemented.  However, the purpose of an abstract class is to act as a base class for concrete classes to derive from. So normally, all we want is just an abstraction without any implementation.

Any concrete class inheriting from this abstract base class, must provide implementations of all the inherited abstract methods. C# complier recognizes an abstract class when we put the keyword abstract in front of the class keyword, and the same for abstract method. An abstract method has no curly braces because it should not have any implementation. And C# complier would only admit the concrete class has an implementation for its inherited abstract method when we put the keyword override to decorate the method. Additionally, the override method should have the same method signature as the abstract method, and we can actually keep the curly braces empty.

>public abstract class BookBase
>{
>	public abstract void AddGrade(double grade);
>}
>public class Book : BookBase
>{
>	public override void AddGrade(double grade)
>	{//concrete implementation code}
>}

It feels like the example above can be achieved more easily with method overload technique, since all we do is to provide different versions of AddGrade() method, and we do that at a cost of creating a whole concrete class for each version of AddGrade() method. However, a class whether it is abstract or not, can contain members other than method, and it is a higher level of polymorphism than method overload. With that being said, we should use it sparingly, as we are making our code complicated and it would only be worthy when other ways fail.

So, what are other ways? What options do we have to achieve this polymorphism in our code?
With an abstract class, we are providing an abstraction for concrete classes to implement, but this is not pure because an abstract class can contain implemented methods. A pure abstraction option would be the interface technique.

A situation where the base class, abstract or not, has an implemented method, and the derived class needs to override it, then using the keyword override to decorate the method in derived class is not enough. We have to use another keyword virtual to decorate the method in base class for that to happen. Same requirement for event.

>public abstract class BookBase
>{
>	public virtual Statistics GetStatistics()
>	{throw new NotImplementedEeception();}
>}
>public class Book : BookBase
>{
>	public override Statistics GetStatistics()
>	{throw new NotImplementedEeception();}
>}

An interface is somewhat like a class, it defines a type, and it can contain most class members. But none of the members has implementation. Well, maybe the simplest syntax of property counts as an implementation. 
By convention, interface name starts with a capital I to indicate this is a interface type. 
Notice none of the members has accessor, that's because when a class uses this interface, it has to have all these members as public members. Now you see why a property can have its simple syntax inside the definition of an interface, because a public field is generally a bad practice. Also, we don't need to specify the method as an abstract method, even though it is one. A class using this interface must have the method as public, but can choose to provide implementation or not, so the class can have the method as an abstract one and that's ok.

>public interface IBook
>{
>	void AddGrade(double grade);
>	Statistics GetStatistics();
>	string Name {Get;}
>	event GradeAddedDelegate GradeAdded;
>}

Now, how does a class use an interface? If the class has no explicit base class, we can put the interface name after the colon. If the class has an explicit base class, we need to put a comma after the base class, and specify the interface name after. A class can only have one base class, but is allowed to use multiple interfaces, we just put another comma and follow with another interface name.

>public class NamedObject
>{
>	public string Name
>	{get; set;}
>}

>public class BookBase : NamedObject, IBook
>{
>	public abstract void AddGrade(double grade);
>	public Statistics GetStatistics()
>	{
>		throw new NotImplementedException();
>	}
>	public event GradeAddedDelegate GradeAdded;
>}

If the base class uses an interface, the derived class would also inherit those interface members. I say inherit not have, so we don't need to have those members in side the definition of the derived class, unless it is an abstract method, then in that case, we need to provide an implementation for it.

Remember in the beginning we said interface defines a type just like a class. A method can choose to take in an object which class implements an interface, but the parameter typer can be the interface type, rather than the class type.

>public class Buffer<> : IBuffer <> // T goes into the angle brackets
>{// ...more code}
>var buffer = new Buffer<>(); // double goes into the angle bracket
>ProcessInput(buffer);
>private static void ProcessInput(IBuffer<> buffer) // double goes into the angle bracket
>{// ...more code}