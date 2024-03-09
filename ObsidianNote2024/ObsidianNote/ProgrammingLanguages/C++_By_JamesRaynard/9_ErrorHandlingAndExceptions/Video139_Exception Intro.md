
In C++, technically any type can be used for an exception object, including built-in types, but the flexibility pays a great price when the code needs to be maintained. So C++98 introduced `std::exception` as the standard interface for exceptions under the `<stdexcept>` header. `std::exception` is a base class, and the library offers many sub classes for many different kinds of errors. For example, `std::out_of_range` is an exception class for accessing memory which doesn't belong to us. One useful virtual member function `what()` returns a C-Style string describing the error. Now, we are still able to use any type for an exception object, but we really should use something derived from `std::exception`.

```
int main() {
	vector<int> vec;
	cout << vec.at(2) << endl;
}
```
The `vec.at(2)` is equal to `vec[2]` plus a bound check. When the check fails like example above, it throws an exception `std::out_of_range`. But we didn't tell the runtime to help find a handler, so the program terminates straight away.

This exception mechanism requires some codes to be executed in runtime, and the compiler will add those codes for us if we tell it to with a try block.
```
int main() {
	vector<int> vec;
	try {
		cout << vec.at(2) << endl;
	}
}
```
Now, when the `std::out_of_range` exception is thrown, the runtime will help to find a handler. But we actually haven't provide one. So no handler found, and program terminates.

To add a handler, we use a catch block. The catch block argument takes an exception. If we use `std::exception`, then this handler code will handle `std::exception` objects and any exception object of derived classes. Therefore, we should take it by reference to use the dynamic type. Otherwise, we get the base class version of `what()` regardless.
```
	catch (const exception& excep) {
		cout << excep.what() << endl; // std::out_of_range version
	}

	catch (const exception excep) {
		cout << excep.what() << endl; // std::exception version
	}
```


We put this catch block right after the try block. That's where the runtime starts to look for handler, in the same scope of the try block. If it cannot find any handler, then it goes to the function caller's scope, and try to find one there. If it keeps failing, it will get to the caller's caller, then eventually the main(). If there is no handler found in main(), then the program terminates.
```
int main() {
	vector<int> vec;
	try {
		cout << vec.at(2) << endl;
	}
	catch (const exception& excep) {
		cout << excep.what() << endl;
	}
	cout << "finish!" << endl; // this gets executed
}
```
Now, the program actually executes till the end, then terminates. Our handler didn't really solve the error, but once the runtime gets through the handler code, it starts executes whatever comes after the catch block. If the error is something beyond fixing or cannot be ignored, then we should tell the program to terminate in our handler code.




