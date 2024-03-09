
A catch block can only come after a try block, but we can have more than one catch block for the same try block. Ideally, we should have one catch block for one static type of exception, but sometimes, we can only fix a few errors, and for the rest, we display the `what()` and terminate the program. So, very likely, we would have a catch block for `std::exception`, and additional catch blocks for exceptions we really can handle.

When this situation happens, the order to write our catch blocks matters. We learnt that runtime starts to look for handler in the same scope where the try block is. A more concrete definition is the runtime starts looking right after the try block, line by line, and if there are multiple catch blocks, then the first one appears after the try block will be used to handle the error given that catch block can handle the type of exception.
```
int main() {
	try {
		vector<int> vec;
		cout << vec.at(2) << endl;
	}
	catch (const exception& ex) {
		cout << "std::exception" << endl;
	}
	catch (const out_of_range& ex) {
		cout << "std::out_of_range" << endl;
	}
}
```
Even though `out_of_range` is a more specific type of exception, the runtime uses the catch block of `exception` because it is the first one appears right after the try block. But we do want the catch block of `out_of_range` to handle it because it can do a better job, or sometimes the right job. So the common practice is to write catch blocks in the reverse order of the exception hierarchy when there are multiple catch blocks.
```
int main() {
	try {
		vector<int> vec;
		cout << vec.at(2) << endl;
	}
	catch (const out_of_range& ex) {
		cout << "std::out_of_range" << endl;
	}
	catch (const exception& ex) {
		cout << "std::exception" << endl;
	}
}
```

Normally, when the program's execution goes into a catch block, it is not in its perfect state. So, we should avoid allocating memory, creating new variables, and calling functions, etc. because all those things can possibly throw a new exception. If we must use variables, then consider built-in types first.

We can nest try catch blocks.
```
try {
	...
	try{
	...
	}
	catch (const std::bad_alloc& ex) {...}
}
catch (const std::exception& ex) {...}
```

Although we now understand putting catch blocks right after try block will mitigate the overhead of exception mechanism, we are allowed to put try catch block anywhere we want. This enables us to deal with all exceptions in one place, preventing error-related codes from spreading all over the program.