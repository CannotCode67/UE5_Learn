
We learnt that a catch block of `std::exception` will catch all standard exception objects including the ones of derived classes. But `std::exception` was added since C++98, and even after, people can write exception with types other than `std::exception`. So, C++ has a syntax to denote a catch block to catch all exceptions, which is three dots.
`catch(...) {cout << "catch all exceptions" << endl;}`

By the way, the exception throwing syntax is pretty easy. We just use the `throw` syntax, and anything after that becomes an exception.
```
int main() {
	try {
		throw 42; // throw an integer as exception
		/*
		throw "something wrong"; // throw a c-style string as an exception
		vector<int> vec;
		vec.at(2);
		*/
	}
	catch(...) {
		cout << "catch all exceptions" << endl;
	}
}
```

This is mainly useful as a fallback catch block because not all people follow the rule to use the `std::exception`. Sometimes, a program can terminate for various reasons. One of them is unhandled exception. If we put this catch all block in main(), then we know if there is any unhandled exception.