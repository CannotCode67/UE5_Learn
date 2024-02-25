In C#, when we invoke a method that requires parameters, we are passing the parameters by value, always, unless we exclusively use a keyword to tell the complier otherwise.

Passing parameters by value means we are taking whatever the variable is, copy the variable's value, and use the copy inside the invoked method. So any change done to that copy would not affect the variable's value.

The console would show an integer of 3, not 5, because the PlusTwo method is just modifying the copy of x's value.

>var x = 3;
>PlusTwo(x);
>Console.WriteLine(x);
>private void PlusTwo (int number)
>{
>	number += 2;
>}

This behavior is straightforward and easy to anticipate when we are passing value type parameters. However, it gets a little bit complicated when dealing with reference type parameters.

The console would show NewBook. And the method SetName is modifying the same object which variable book1 is pointing to. You might wonder aren't we passing parameters by value. Yes, we are still passing by value, it is just the value happens to be the memory address, and the copy of that value contains the same memory address, thus the method accessed the same object through a copy of a reference. It is very much like having two different variables sharing the same reference value, and we access the same object no matter which variable we use.

>var book1 = GetBook("Book 1");
>SetName(book1, "NewBook");
>Console.WriteLine(book1.Name);
>private void SetName(Book book, string name)
>{
>	book.Name = name;
>}

In other languages where we are passing by reference, the method actually receives a reference value to the variable, and through the reference value that variable holds, accesses the object. However, in this case, we can even modify the variable's reference value, and that's not possible for passing by value scenario.

Now, let's see another example with reference type parameters.

The console would show Book 1. Well, yes, we are passing by value, so we take the reference value of variable book1, and make a copy, then pass the copy into the method GetBookSetName. The pitfall is we think we are accessing the same memory address, thus creating a new object in that location, which should change whatever variable book1 is referencing, because they are accessing the same memory location, right? If we really are accessing the same memory location, then yes, but we are not. The operation of creating a new object using the keyword new, in this case, returns a new reference value, and that reference value is assigned to the parameter variable. Before this operation, the parameter truly holds the same reference value as variable book1. But after the operation, we are ended up with a new reference value leading to a new object, so any modifying after that has nothing to do with variable book1 and its referencing object.

>var book1 = GetBook("Book 1");
>GetBookSetName(book1, "NewBook");
>Console.WriteLine(book1.Name);
>private void GetBookSetName(Book book, string name)
>{
>	book = new Book(name);
>	book.Name = name;
>}


In the beginning, we talk about we can exclusively tell the complier to pass by reference by using a keyword, that is the keyword ref.

The console now shows NewBook, due to the fact we are telling the complier to pass the book1 as referene. Be aware we need to use ref keyword when we define the method also when we invoke the method. The intension is to make whoever using the method know the method is passing by reference. Again, we are not accessing the memory location and create a new object there. Instead, we are returning a new reference value when we create the new object, but this time, the new reference value is assigned to variable book1.

>var book1 = GetBook("Book 1");
>GetBookSetName(ref book1, "NewBook");
>Console.WriteLine(book1.Name);
>private void GetBookSetName(ref Book book, string name)
>{
>	book = new Book(name);
>}

To confess, there are two keywords which can tell the complier to pass by reference, not one. We already study keyword ref, and the other one is keyword out. They are very similar, but with keyword out, the complier assumes the referenced variable has not been initialized, therefore requires a value assignment inside the method.

The console still shows NewBook, but if the opertation of creating new object and assigning the reference value to the referenced variable is missing inside the method, then this would not complie. It doesn't matter whether we already assign a value to it before invoking the method. One more thing to notice, is we still need to write out the keyword out when invoking the method even we already did when defining it, just like keyword ref.

>var book1 = GetBook("Book 1");
>GetBookSetName(out book1, "NewBook");
   Console.WriteLine(book1.Name);
>private void GetBookSetName(out Book book, string name)
>{
>	book = new Book(name);
>}