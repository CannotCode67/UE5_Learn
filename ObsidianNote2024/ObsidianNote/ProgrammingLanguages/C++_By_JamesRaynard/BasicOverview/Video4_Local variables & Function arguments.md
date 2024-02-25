
Local variables exist inside a scope, and a scope is defined by a pair of curly braces `{}`. When the scope ends, the local variables inside that scope get destroyed, meaning we no longer has access to them. The destroy order is the reverse order of execution, so it starts from the end.

A local variable comes into existence when it is defined. How to define one? Like this.
```
{
	int i;
}
```
When it is defined, memory is automatically allocated for it on the program's stack.
It looks like missing initialization, but it is not. Without specifying an initial value, it is default initialized. For built-in type like int, it uses whatever value happens to be in that memory address. For classes or structs, the default constructor is called.

C# is quite different, as built-in types usually have specific default value if no initial value is given. For example, int variable would be zero. For classes, the only way we can call their constructors is using the keyword `new`, and they live in heap, not stack. In C++, if we want the class object to exist in heap, we use the keyword `new` as well. So, maybe C# don't have the ability to initialize a class object in stack?

By default, variables passed into a function are passed by value, so they are copied. Those copies are local variables whose scope is the function body. By default, the return value is also a copy.
```
using namespace std;

int SomeFunction(int y) 
{
	cout << &y << endl;
	return y;
}

int main()
{
	int x = 2;
	cout << &x << endl;
	int z = SomeFunction(x);
	cout << &z << endl;
}
```
The example above shows variables x, y, z all have different addresses, but the same value.

Passing by reference is how we call the alternative in C#.
Passing by address is what we call in C++, as we can pass by pointer or pass by reference, but under the hood, we are passing the memory address of the variable.

Pass by pointer
```
using namespace std;

int SomeFunction(int* y) 
{
	cout << y << endl;
	*y = 1;
	return *y;
}

int main()
{
	int x = 2;
	cout << &x << endl;
	int z = SomeFunction(&x);
	cout << &z << endl;
}
```
The example above shows x and z have different addresses, because z is still a copy. The pointer variable holds a value of memory address, so variable y is a variable of int pointer(`int*`). We are passing the memory address of x, so y's value is equal to &x. We use `&` sign to get the memory address out of a regular variable. And to reach the object through a pointer, we dereference the pointer by putting a `*` sign in front of the pointer variable.

Pass by reference
```
using namespace std;

int SomeFunction(int& y) 
{
	cout << &y << endl;
	y = 1;
	return y;
}

int main()
{
	int x = 2;
	cout << &x << endl;
	int z = SomeFunction(x);
	cout << &z << endl;
}
```
The example above shows x and y have the same memory address, and z has a different one because z is still a copy. Under the hood, reference is implemented as a pointer which automatically gets dereferenced by the compiler. Directly modifying reference variable(`int&`) is modifying the object, and to retrieve the memory address, we use `&` sign like we do for the regular variable.

const reference
```
using namespace std;

int SomeFunction(const int& y) 
{
	cout << &y << endl;
	y = 1;
	return y;
}

int main()
{
	int x = 2;
	cout << &x << endl;
	int z = SomeFunction(x);
	cout << &z << endl;
}
```
The example above would get a compiler error as we change the value of a const reference.