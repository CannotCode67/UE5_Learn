
The best guarantee of an exception safety is it will not throw any exception. But we all know if an exception is properly used, then there is usually a situation beyond our control. Yet, it is still helpful to know some codes do not throw an exception, so that we have some foundation to work with. None of the functions and operators in the core C++ language and standard library throws exceptions, apart from `new`, `dynamic_cast`, and `throw`.

How do we indicate some codes will not throw an exception? C++98 provided the throw() exception specifier used with function.
```
void SomeFunction() throw(std::out_of_range, std::bad_alloc) {
	...
}
```
This means this `SomeFunction()` can throw exceptions in the following types `out_of_range` and `bad_alloc`. If we provide no argument for the throw() specifier, then it means the function doesn't throw any exception, a way to indicate some codes with no exception guarantee.

The problem would be there is no way to check against this exception list. The best a compiler can do is to check if the exception in the list is defined or not. And if the function does throw an exception which is not on that list, then the program will terminate right away without attempting to find any handler. So the person who writes the list must be perfectly good at it, otherwise this throw() exception specifier is just another way to crash your program.

It is unpopular, so throw() exception specifier was deprecated since C++ 11 and removed from C++20. We should know it in case we encounter some old codes, but we should not use it.