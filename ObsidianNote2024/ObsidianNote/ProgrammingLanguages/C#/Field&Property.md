In C#, field and property are two different things, but this is like array and list have all kinds of different definitions across languages. In other languages, field and property might just be the same thing.

Now, field member is one the most common member for a class. We can use accessor to control whether we can access the field from outside the class. However, there are times when we want the field to be readable and retrievable from the outside but not changeable. For example, we want an user to be able to get the name of a book instance, but don't want the user to directly set it since he can set it to null or whatever value that is just inappropriate. In cases like this, an accessor is just not enough.

A way to fix this is to set the field private, and write two different public methods to handle the behavior. Getting the name is easy, just return the field. Setting the name through a method allows us to write validating code to make sure the input is valid before assigning it to the field.

>class Book
>{
>	private string name; // field
>	public string GetName() // method 1
>	{return name;}
>	public void SetName(string newname) // method 2
>	{
>		if(!String.IsNullOrEmpty(newname))
>		{
>			name = newname;
>		}
>	}
>}

A property in C# is designed to work just the same, but with a more concise syntax. And along the development of C#, new syntax for property is added, so there are a couple of ways of writing a property.

For property, an user from outside the class accesses it just like accessing a field.

>book.Name = "newBook";
>Console.WriteLine($"The book name is {book.Name}.");

Following is the longer version of property syntax, where we need to explicitly define a field outside the property syntax. 
The property is like a field but with implementation inside curly braces, and is like a method but without parameter. By convention, it should have the same name as the related field but with a capital letter for the first character. The get part is called getter, and the set part is called setter. When an user retrieve a property, the getter is invoked, when he assigns a value to the property, the setter is invoked. The property has a public accessor, so the getter and setter would automatically have the same accessor of public, unless we explicitly put a different one in front of the keyword get or set.
Also the value is an implicit variable storing the actual value passed to the setter. Like it should have a string value of newBook in our example.

>public string Name
>{
>	get
>	{return name;}
>	set
>	{
>		if(!String.IsNullOrEmpty(value))
>		{name = value;}
>	}
>}
>private string name;

Another way of writing property is like this. It is a simplified version, as it assumes all the getter does is to return a value from a field, and all the setter does is to assign a value to a field. Then, the C# runtime would automatically generate a field with the same name of the property behind the scene, so you don't need to do it. Inside the class, we can access this field using the property name just like we access a regular field.
If both the getter and setter are public, then this property is no different from a public field. But, we can still put a different accessor in front to suit our need, like our example. Doing it this way effectively makes the property read only because we can read the value but never change it from the outside. If we want a public setter but with code to validate input, we have to use the longer version syntax.

>public string Name
>{
>	get;
>	private set;
>}

If we are after the read only functionality, then using a property like this is one way, but there are different ways, and one of them is the keyword readonly.

Readonly keyword can be added before type and after accessor. Fields with readonly keyword can only be initialized like this, or changed in a constructor.

>public class Book
>{
>	public readonly string bookName = "";
>	public Book(string name)
>	{
>		bookName = name;
>	}
>}

Another keyword to make a field read only is the keyword const, standing for constant. It has a stricter rule than readonly. A const field can only be initialized with a constant value, after that the contained value cannot be changed, not even by a constructor. A constant value has to be hard-code value, not a parameter, not a variable, etc.
If you think about it, a constant is the opposite of a variable, it would make sense that we can only initialize it. 
If someone outside the class wants to access the constant, then he would need to use the class name to access the constant, not the object name. The reason would be a const member is a class member, not an instance member, just like static member. 
Think about it this way, if you initialized a const field with a constant value in a class, it cannot be changed, so all objects of this class would have the same value for this field, then why do we need each object to carry a piece of information that is the same across. To not waste resourse, a const field is a class member. And by convention, a const would have name of all upper cases, so others can tell it is a const value at a glance.

>public const string BOOKNAME = "Science";

We cannot change a const field in a constructor, but we can initialize one in a constructor. However, const field initialized in a constructor has to be private, otherwise, it would not complie. 
Therefore, the const field we can access are public for sure, and it must be initialized outside of a constructor, a piece of information carried by the whole class. A const field that is initialized inside a constructor must be private, and it is a piece of information carried by object, not the class, and we have no way to access it.