So far, we know when something goes wrong with our code, we cannot complie, but there are situations where we get the code complie, but things can still go wrong. Especially when application takes in user input, users can enter values that are expected, also values that are not expected. And an unexpected value can easily crash our program. Exception is something unexpected happens for the program, as its own name suggests.

Exception in C# is a type, a very basic type for many concrete exception types to inherit from. And .Net has offered some built-in concrete exception types like NullReferenceException, ArgumentException, and FormatException, etc. If without them, something goes wrong with our program, the program would crash without any message. Now, at least we know what type of error happens. And better, we might be able to prevent the program from crashing by handling the exception.

The line of code: Console.WriteLine("Invalid value") is a way of telling the user the value doesn't match the requirement. It works just like an exception.

>public void AddGrade(double grade)
>{
>	if (grade <= 100 && grade >= 0)
>	{grades.Add(grade);}
>	else
>	{Console.WriteLine("Invalid value");}
>}

But an exception can provide more information in a more sophisticated way. For exception, we have to use the keyword throw. This is an argument exception, so we use the corresponding exception type. We can also pass string value to show more information. The operator nameof() can take in a field name, a method name, a parameter name, any identifier exists in the scope, and return a string representation of that identifier.  So, we now enter a grade of 105, the program would crash and show a message: Unhandled Exception: System.ArgumentException: Invalid grade.

>public void AddGrade(double grade)
>{
>	if (grade <= 100 && grade >= 0)
>	{grades.Add(grade);}
>	else
>	{throw new ArgumentException($"Invalid {nameof(grade)}");}
>}

You might think then why do we need exception? Throwing an exception produces the same message to the console just like Console.WriteLine(), and it crashes the program. Yes, when an exception is thrown, the runtime is looking for a catch statement to handle the exception, and it starts looking first inside the method where the exception is thrown, if nothing found, it moves on to the caller, if nothing found still, the runtime would crash our program.

When we know we are dealing with codes that might throw an exception, we should use the try catch syntax. 
Here, we take the user input and parse that input into a double precision number. If the input is not number in string, then there is no way to parse that into a double precision number and we get an exception. AddGrade() method also throws an exception when an invalid grade is passed, just like we write it above. 
Those codes are now inside the curly brace of try statement, and if they do throw an exception, the execution would jump to the catch block of code. If the parsing throws an exception, the execution would skip over the AddGrade() method, and jump right into the catch block of code. By catching the exception, the program would not crash, it would continue its execution flow. If it is in a loop, it would go into the next iteration. 

>try
>{
>	var grade = double.Parse(input);
>	book.AddGrade(grade);
>}
>catch(Exception ex)
>{
>	Console.WriteLine(ex.Message);
>}

Also, we mention Exception is a very basic type, by using Exception here, we are basically catching all exceptions, while in reality, we only want to catch some specific exception types. More specific, we only want to catch exceptions that we know we can safely handle, otherwise, a crashing halt might just be the right thing to do. And if something we cannot handle happens, and we still need to execute some code before the program crashes, such as closing a file, closing a socket, etc.  We can use the finally keyword to contain some code that would be executed regardless we successfully catch the exception or not.

>try
>{
>	var grade = double.Parse(input);
>	book.AddGrade(grade);
>}
>catch(ArgumentException ex)
>{
>	Console.WriteLine(ex.Message);
>}
>catch(FormatException ex)
>{
>	Console.WriteLine(ex.Message);
>}
>finally
>{
>	Console.WriteLine("**");
>}


Now, we should answer the question, why do we need the whole exception throwing and catching thing? Why can we just validate the input and use a Console.WriteLine() if it is invalid? Becasue it can also prevent the program from crashing while displaying valuable message, without the whole throwing trying catching thing. Well, actually, we can just use a Console.WriteLine() in that case. 
From conceptual standpoint, when something unexpected happens, we want to explicitly declear to the user that an error condition just happens. And we want to separate this type of things from the other regular things, so we create an exception type, and all the concrete exceptions, treating them differently with special syntax.
Another point about the whole exception thing is to provide a chance to let someone do something about the exception somewhere else in the code, usually the highest level. In reality, a project is developed by multiple programmers, and usually there is one guy responsible for dealing with all kinds of exception. And he does that by using a try catch block probably in main(). Other members of the team just throw the exceptions. Now this approach is not the absolute right approach to go with, we can still write our try catch block to solve the exception in our method, or even use validating code to prevent an exception from happening. It's just without the exception thing, we lose the possibility to defer this error handling.