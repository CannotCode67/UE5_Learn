In C++, traditionally, there are four special member functions: constructor, copy constructor, assignment operator, and destructor.

Constructor
The constructor has the same name as the class, and if we don't write one, the compiler would provide a default one. In C++, we can call the constructor with or without the keyword new, as a result, the object would live in stack or heap. C++ allows the initialize syntax to directly assign the value when the object is constructed, which is shown below.
The class cpp file.
```
Test (int i, const string& str) : intMember(i), strMember(str) {}
```
The main file.
```
int Main()
{
	Test t1(12, "TestNumber");
}
```
Now, what is this t1 variable? The object itself? or it is just a pointer? Need to figure out. I think it is the object itself instead of the memory address of the object.

Copy Constructor
The copy constructor is similar to the constructor, but it is using another object for initialization. If we don't write one, the default copy constructor would always have one argument, the const reference of another object.
The class cpp file.
```
Test (const Test& another) : intMember(another.intMember), strMember(another.strMember) {}
```
The copy constructor is automatically invoked when an new object is created from an existing object of the same kind.
```
Test test1(x, y);
Test test2{test1}; // here the copy constructor is invoked
Test test3 = test1; // here the copy constructor is also invoked
```
In example above, both `test2` and `test3` is created right on their own line. And `test3` might be confused with assignment operator below. That's why copy constructor is sometimes called copy assignment constructor. The key difference is assignment operator only works for an existing object. Here, `test3` object doesn't exist before that line.

Assignment Operator ( = )
Conceptually, the assignment operator may seem to be very much like copy constructor. And sometimes, some people call it copy assignment operator. The key difference is when we invoke the copy constructor, the object is not existed yet. Only after the copy constructor, the object is alive. However, assignment operator works on an existed object only.
The class cpp file.
```
Test& operator= (const Test& another)
{
	intMember = another.intMember;
	strMember = another.strMember;
	return *this;
}
```
Because the object already exists, so we cannot use initialize syntax. `this` is the pointer to the object, and `*` in front would dereference it. When return, the compiler would automatically take this object and return the reference of the object.
The main file
```
int Main()
{
	Test t1(12, "TestNumber");
	Test t2;
	t2 = t1; //here it invokes the assignment operator.
}
```

Destructor
If the object lives in stack, when the execution goes out of the scope, the object is automatically destroyed by calling its destructor implicitly. The other situation where the object lives in heap, we need to use the keyword `delete` on the pointer without dereference to explicitly invoke its destructor to destroy the object. 
The class cpp file
```
~Test() {}
```
Destructor is like constructor, in the sense of how it names. The `~` sign (pronounced tilde) follows by the class name is how the destructor is named.