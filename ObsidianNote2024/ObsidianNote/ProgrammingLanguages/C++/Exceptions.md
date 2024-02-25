Exceptions are for things go wrong. That idea holds true in C++. The standard library provides a couple of written exception class for you. And the way we invoke one is to use the keyword `throw`, very similar to C#. The reason for having exception is the language treats it specially. If we don't have exception, when we run into something unexpected, we would struggle to pass that information back in a situation where a long chain of calls exists.
```C++
if (num == 0)
{
	throw std::invalid_argument("number cannot be zero!");
}
```

People who are responsible to catch this exception would write
```C++
try
{
	StartNumberGame(num);
}
catch (std::invalid_argument& ex)
{
	//handle the error.
	cout << ex.what();
}
```
So we can access the string value we enter by calling its `what()` function.

// clean up
When the exception is thrown, the execution goes to the catch block if it can find one. Then, any code after that line throw an exception and before the catch block would get skipped. One of the bad situations this behaviour can get us into is skipping the clean up code. What is a clean up code? If you open a file, do whatever you want, then when you finish it, you need to close the file. If you obtain an internet socket, in the end, you need to close that as well. But the exception just skips all the code right? Then how can you do clean up? The good practice is to use constructor to obtain resource, and implement the destructor to do the clean up. While exception is thrown and looking for catch block, the system also does something called unwiding the stack, which fires the destructors for objects out of scope. That's why using destructor to do clean up is such a great idea.

We can check what exception class is available in the standary library through the cppreference.com.

Exception is thrown, but execution now needs to find a catch block, starting from the place where exception is thrown. If it fails to find one, it goes to the caller, and start to look there, if not found again, it continues the process until it hits the `main()` and explodes. This process is slow. So when exception is thrown, there is a huge performance hit.