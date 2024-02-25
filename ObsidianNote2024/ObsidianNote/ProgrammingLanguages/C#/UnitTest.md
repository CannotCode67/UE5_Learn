Unit test is first of all a test, to find out if the code behaves like expected. Unit test is a test with a scope of small units of code, a method for example. Doing tests this way is good because in object-oriented language, we often encapsulate our code into pieces. And if a small piece of code can pass the test, then we don't need to worry about this piece of the code and begin writing more code on top of it.

C# like other major programming languages has test library available, and .Net provides a test runner functionality to make the testing process automated. So far, we are actually running tests manually. Each time we write some code, and go to the CLI to run the project, we are testing the code with an expected result in our mind and see if the CLI shows the same result. By automating the process, we can write many unit test codes, and tell the CLI to run all the tests in one go, so we don't have to do it one by one manually. On the other hand, test librarby offsers classes and methods that we can use when writing unit test codes.

 By convention, unit tests are put into a different folder as a different project. The production code is put into a folder called src, and the unit tests are put into a folder called test. Those folder names are also by convention. Then, we go to CLI to use dotnet command to build a unit test project. Different test libraries use different project templates, and the one we use is Xunit. The CLI command is dotnet build xunit.

Xunit library is not a part of the .Net core, so we need to go to NuGet to get it. Xunit marks the test method with an attribute [Fact], and only those marked methods are treated as tests during test run. Assert is a class of Xunit library, offersing many static methods to do logical tests. And the triple-A way of seting up a test is to divide test codes into arrange, act, and assert.
>using Xunit;
>namespace GradeBook.Tests
>{
>	public class BookTests
>	{
>		[Fact]
>		public void Test1()
>		{
>			// arrange
>			var book = new Book("");
>			book.AddGrade(89.1);
>			book.AddGrade(90.5);
>			book.AddGrade(77.3)
>			
>			// act
>			var result = book.GetStatistics();
>			
>			//assert
>			Assert.Equal(85.6, result.Average, 1);
>			Assert.Equal(90.5, result.High, 1);
>			Assert.Equal(77.3, result.Low, 1);
>		}
>	}
>}

The way to use .Net's test runner is to go to CLI, and use command dotnet test.

And because the unit tests are in a different folder, when we try to access the mehtod in production code, we have to add the reference into the test project, just like external dependenies on NuGet. The way to do this is to use CLI command dotnet add reference, with the system path to the production project file.

As we can see, the asset section of unit test is kind of hard coded, meaning we are giving a manually calculated result to the code to compared with. Also, for floating number, we need to decide how many decimal places we need to compare, otherwise we just never pass the test.
