Reference type and value type are two different ways of handling variables, and they are both very common in not just C# but other object-oriented programming languages.

If a variable is a reference type, then this variable is a pointer containing the value equaling to the memory address to the memory slot where the actual object is stored. Types defined through classes, are reference types, because usually these types are more complex and need to be stored in a bigger memory slot.

Because Book is a type we define through a class, the variable b only contains the reference to the actual object, but not the object.

>var b = new Book("Grades");


If a variable is a value type, then this variable directly contains the value. Types defined through structs, are value types, because usually value types are simple and requrie a small memory slot to be stored.

Because 3 is an integer, and integer is a type defined through a struct, variable x directly stores the integer value instead of a reference.

>var x = 3;


The whole idea of handling them differently is efficiency. It makes no sense having the variable to maintain a reference value to the actual value since most of the time, the actual value might not even as big as the reference value. However, class objects are complex and big, and it would make sense to use a reference, and this additional layer creates the possibility where we might be able to solve the problem just by manipulating the reference value.

Variable book2 has the same reference value as variable book1, so book2 is now a pointer to the same object which book1 is pointing to. There is only one object even there are two different variables pointing to it.

>var book1 = GetBook("Book 1");
>var book2  = book1;

Variable x has an integer value of 3, so it is a value type, when we assign x to variable y, we are taking the x's value, make a copy, then assign the copy to variable y. So, any change done to y would have nothing to do with x, because y's value is a copy.

>var x = 3;
>var y = x;


We mention reference type and value type are defined through class and struct, respectively. Class is a very familar component of C# language, struct however, is less known. But, they are more similar than you think. When you define a struct, you give it an accessor, followed by the keyword struct instead of class, and lastly the name of the struct.

>public class Person
>{}
>public struct Point
>{}

And we can give it property member, method member, just like class. But we now know that struct is value type, and it would not be wise to define a huge struct type, as value type is only suitable for small simple things.

If we need to find out whether a type is defined through a struct or a class, we can move the cursor to where the type is, and press F12 to enter a metadata mode. Visual Studio Code or Visual Studio offer this metadata view, it is like the reverse engineering of the code, but it is not gonna give us the concrete implementation, but at least it would tell us whether the type is defined by a struct or a class.