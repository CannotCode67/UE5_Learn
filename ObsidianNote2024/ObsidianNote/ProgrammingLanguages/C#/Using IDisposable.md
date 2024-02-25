We have learnt the interface technique, and one of the very common interfaces is IDisposable. It only contains one method, the Dispose(). Of course, someone needs to implement the method, but generally, the purpose of this method is to close a file, or a network socket, or something similar. Many built-in classes in .Net core library have this interface implemented. Becasue when a file is opened, the file is locked and cannot be opened again. So we need to write code to close the file after we are done using it. The Dispose() method is a popular choice for this kind of work when the class uses IDisposable interface.

>public override void AddGrade(double grade)
>{
>	var writer = File.AppendText($"{Name}.txt");
>	writer.WriteLine(grade);
>	writer.Dispose();
>}

However, if something happens in writer.WriteLine(grade), throwing an exception, then writer.Dispose() would not be executed, leaving the file opened. From what we learn, we would use a try-catch-finally block to make sure the Dispose() method gets called. Well, there is a pattern associated with this kind of situation, a simplified syntax if you may.

The using statement is overloaded, it can bring in namespace, and it can be used in this pattern. This syntax basically tells the complier to open the file, and try the code in curly braces, and invoke the Dispose() method in a finally block. In this syntax, the Dispose() method is guaranteed to be called, and it is done implicitly. However, the syntax only works for classes using IDisposable interface.

>public override void AddGrade(double grade)
>{
>	using(var writer = File.AppendText($"{Name}.txt"))
>	{
>		writer.WriteLine(grade);
>	}
>}

