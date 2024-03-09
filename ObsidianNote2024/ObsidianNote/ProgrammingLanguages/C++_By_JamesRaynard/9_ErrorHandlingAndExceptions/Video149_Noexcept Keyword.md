
The throw() exception specifier is removed, but C++11 introduced another keyword `noexcept` to mark the function with no exception guarantee.
```
void SomeFunction() noexcept {
	...
}
```
It behaves similar as an exception is thrown, then the program terminates right away without attempting to find a handler.

So it is a promise made by the function writer to the function caller. The benefits are when compiler sees this keyword, it can generate better optimized code because that's one more assumption given to the compiler to squeeze more performance. The standard library uses only the operators which are declared `noexcept`. And in fact, modern C++ has optimized versions of some operators which may throw exception. The standard library just call the old versions of them to be safe.

We should declare a function `noexcept` when we are certain it will not throw any exceptions. Or we could leverage the immediate termination of `noexcept` to kill the program as soon as possible before things get worse.

Once a function is marked `noexcept`, it cannot be overload with a version which is not marked `noexcept`. It is part of the function's type but not part of the function's signature, similar to the return type.

If a virtual function is `noexcept` in the base class, then overrides in the derived class must also be `noexcept`. If a virtual function is not `noexcept` in the base class, we can still mark the overrides in the derived class with `noexcept`.

Inheriting constructors will be `noexcept` if the base class constructor is `noexcept`.

If all members of the class have `noexcept` destructors, or they are built-in types, the compiler will assume the class's destructor is `noexcept`. Also if all parent classes have `noexcept` destructors, then the compiler will make that assumption as well. This allows existing code to have the `noexcept` benefits automatically.