Delegate is a type, like int, float, or type defined by a class. 

However, struct types like int, float, etc describe or contain value. While class types describe or contain values plus methods. Delegate is a kind of type that is in between, it describes or contains only methods. 

Defining a delegate is somewhat like defining a method. For example: 

>public delegate string TextHandler( string text); 

-This is done for defining a delegate, the delegate signature is return type of string, and taking in one string parameter.  

-The difference is the delegate keyword, and there is no implementation. 

The reason there is no implementation would be delegate is just a type. A delegate type variable can contain methods that match this delegate type, and the implementation is inside each contained method when we define those methods. 

For example: 

>int number = 3; 
>TextHandler handler = ReturnMessage; // longer version: TextHandler handler = new TextHandler(ReturnMessage); 
>string ReturnMessage(string message) 
 {return message;} 

-We define a method called ReturnMessage. 

-The delegate type is named TextHandler, and a variable of this type is created, called handler. 

-It is just like the integer initialization above, a type, a variable of the type, and the contained thing that matches the type. 

When we define delegate, we define the delegate signature, containing the return type and the take-in parameter number and their types. In our TextHandler example, the delegate signature is return type of string, and taking in one string parameter. Methods that have the same return type, the same take-in parameter number, and the same take-in parameter type as the delegate signature, would be considered containable by a variable of that delegate type. In our example, the method of ReturnMessage has the return type of string and one take-in string parameter, so  it can be contained by handler variable. 

Now, we can invoke the contained method using the variable with a (), like this. 

>string result = handler("Hello"); 

-We have to pass along a string value as parameter like calling the method. 

-The thing is the variable can contain multiple methods, even the same method but multiple times. And this is called multicast delegate. 

This is the whole process from defining the delegate, to initialization of the delegate variable, to assigning the matching method to the variable, to invoking the contained methods using delegate variable. 

But why go through all these to just invoke methods? Well, think of a scenario where the project is developed by multiple people. When my code does thing considered to be significant event, other parts of the project need to do something about it. However, I don't have other developer's codes or even worse, they haven't written any code. A solution is to use delegate to broadcast the significant event. Make the delegate variable public, so other developers can assign their methods to this variable, and be sure about their methods would be called when significant events happen. 

However, we are not quite there yet, because for a delegate to become an event, we still need some work. First of all, when we define a delegate for the usage of event, the convention is to have two special parameters, like this. 

>public delegate void GradeAddedDelegate(object sender, EventArgs args); 

-object type is the base type of all in C#, much like monobehaviour class in Unity, so sender can be any type, but still fit the this parameter. 

-EventArgs is a build-in class of C#, it is designed to send related information out regardless of the type of the information. 

Second of all, when we initialize the delegate variable, we need to add the event keyword, like this. 

>public event GradeAddedDelegate GradeAdded; 

-adding the event keyword would tell the C# complier to add some restrictions to this variable, such like we cannot directly assign a method to the variable using "=", we have to use "+=" or "-=". Also, we can invoke the variable even when there is no method assigned, which is not possible when using pure delegate. 

Still, it is good to check if there is any method contained inside the delegate variable before broadcasting. 

>if(GradeAdded != null) 

  {GradeAdded(this, new EventArgs());} 

That is it, the contained method in event is usually called handler, but other than that, they are just methods matching the delegate signature like what we see in pure delegate scenario.