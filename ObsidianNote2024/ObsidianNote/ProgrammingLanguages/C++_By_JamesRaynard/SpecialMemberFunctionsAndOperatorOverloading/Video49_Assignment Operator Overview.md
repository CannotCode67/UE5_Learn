
Go to Video8 to review the assignment operator basics.

Below is something new.
The assignment operator is a member function, not a special constructor.
It is invoked when we assign two objects of the class.
`y = z;` where y and z are the same class, and already declared.
The alternative form for above is `y.operator=(z);`
We can chain assignment operator. So `x = y = z;` is allowed and `x` and `y` will have the value of `z`. It is executed from the right to the left, so if we write it in alternative form, we would get something like this: `x.operator=(y.operator=(z));`
If we don't write one, the compiler will provide one for us.
```
class Test{
	int i;
	string str;
public:
	...
	Test& operator=(const Test& arg) {
		i = arg.i;
		str = arg.str;
		return *this; // this how to return a reference
	}
}
```
To return a reference, we dereference `this`. `this` is a hidden pointer pointing to the object itself, so dereferencing it would get the object itself. From there, compiler can work out how to get the reference.

When do we need to write one? The default one provided by compiler is good usually, but again when a class manages resource, we need to write one. For assignment operator, two already existed objects must have their own resources bind. If we use the default assignment operator, we get two objects now binding to one resource, just like the scenario with default copy constructor. However, here it is worse, as we lose the binding to one of the resource, be it the heap memory, internet connection, or a file. Thus, that resource now becomes uncontrollable.

