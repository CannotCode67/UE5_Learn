C# is an object-oriented programming language, in which most things happen inside of a method, a member of a class. A class defines a type, the type's methods, and the type's fields. We invoke code through a type, and its methods. Code that loosely floating around outside of a method cannot be invoked, that's why we write our code inside of a method.

This object-oriented approach encourages encapsulation, where we try to categorize blocks of codes into different classes. And with proper naming of those classes, we can divide the program into pieces, making it modular and easier to read.

When we define a class, it is possible the class's name is conflicting with class defined by others. The way we make it unique, is to use namespace. It is simialr to the file system, in the same directory we cannot have two files having the same name, but if those files are in different directory, then it is ok.

We can define a namespace using the keyword namespace and its name, anything inside its curly braces is contained by this namespace.

>using System; //to use content of a namespace
>
>namespace GradeBook // to add content to a namespace
>{
>	class Program
>	{
>		static void Main(string[] args)
>		{}
>	}
>}

By convention, we use one file for one class, but putting all classes into one file is syntactically doable. If there is another class on a seperate file but need to be contained in the same namespace, we can use the same syntax, wrapping the class with the namespace.

Class constructor, is the method we call when we create an object of that class type. C# complier by default would provide an implicit constructor behind the scene, but we can create our own constructor to control the initialization of the object. And beware, once we create our own custom constructor, C# complier would no longer provide the default parameterless constructor. If any code expects the class to have a parameterless constructor, it would break, so we might just need to explicitly write the parameterless constructor in that case.

Constructor method has to have the same name as the class, and have no return type. Often, we put the class properties initialization inside the constructor method.
>class Book
>{
>	double[] grades;
>	public Book()
>	{
>		grades = new double[3];
>	}
>}

We can see the keyword public, and it is one of the access modifiers. Others are private and protected.

Constructor can be implicit, and so does variable. An object has a implicit variable called "this", storing the reference to the object itself.

In cases where we have to explicitly refer to the object itself, we use the variable "this". Without the variable "this", C# complier would think you are assigning the method parameter to the method parameter, which makes no sense.

>class Book
>{
>	private string name;
>	public Book(string name)
>	{
>		this.name = name;
>	}
>}

Now, let's examine two most common class members: method and field.

We define a field with an accessor, a type, and the field name, and we define a method with an accessor, return type, method name, take-in parameter, and implementation code in curly braces.

>class Book
>{
>	public string name; // field
>	public string GetName() // method 1
>	{return name;}
>	public void SetName(string newname) // method 2
>	{name = newname;}
>}

We access class field and invoke class method through the instance name with a dot sign. Of course, there are static members where we need to use a class name instead of an instance name.

>book1 = new Book("Book 1");
>book1.name = "Book 2";
>book1.SetName("Book 3");