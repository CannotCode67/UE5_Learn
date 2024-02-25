
Go to Video8, and we have a simple overview about copy constructor.

Below content is not included in Video8.
Copy constructor is a special version of constructor, so if we write a copy constructor, the compiler will not synthesis any constructor for us, meaning we have to write a default constructor as well.
The copy constructor may also be invoked in function calls.
When we pass an argument by value, a new object is created on the called function's stack which is a copy of the argument.
When we return a local variable by value, a new object is created in the function's return space which is a copy of the variable.

The reason we say may is because compilers can perform optimizations which reduce the amount of copying in function calls, so copy constructor may not necessarily be called.

We don't write a copy constructor for our class, the compiler would provide one for us. This default copy constructor will copy all the data members, if they are built-in type, then just copy the value. If they are classes, then this will call those classes' copy constructors.
```
class Test {
	int i;
	string s;
public:
	...
	Test(const Test& arg): i(arg.i), s(arg.s) {} // this is the default copy constructor
}
```

The default copy constructor is good enough usually. But if the class manages a resource of some kind, then we have to write one. Otherwise, this default copy constructor would give us an copy object binding to the same resource, and now we have two objects interfering each other, stomping on the same resource.