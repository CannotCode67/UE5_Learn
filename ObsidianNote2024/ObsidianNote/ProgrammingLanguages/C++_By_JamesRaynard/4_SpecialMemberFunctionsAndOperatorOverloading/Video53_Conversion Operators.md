
A class can define a conversion operator which is a member function which converts an object to some other types. The conversion can be defined by however we want.
```
class Test {
	int i{42};
public:
	operator int() const { return i;}
}
```
The name of the function is `opertaor int()`, and `const` there to make the object unmodified. The return value is a member field. So, logically it doesn't seem to make any sense, but it doesn't have to. Technically, we can define however we want. On the other hand, the built-in types' conversion operators are designed to make sense, so that people can deduce the return values because conversion operations can happen implicitly.

Implicit conversion
```
Test test;
int x = test + 5;
```
When compiler sees those codes, it first tries to find an exact match, so a plus operator defined to take a Test object and a `int`. Of course it fails. Next step, the compiler tries to convert the arguments. `int` has no defined conversion operator to become Test, but Test class has a conversion operator to become `int`. So that line of code will be called as:
`int x = test.operator int() + 5;`

Another example is `cout << test << endl;`. The compiler tries to convert `test` into something that can work with the `<<` operator. So the output displays the `i` member field of the object.

Implicit conversion may not be what you want. The below example will compile in older versions of C++.
```
int i = 99;
cin << i;
```
The stream object have a defined `operator bool()` to check if the state of the stream object is valid. And we know Boolean value can be converted to `int`. Lastly, there is a `<<` operator called left shift operator that works with two `int`s. The compiler is allowed to convert something multiple times, so here from stream object to `bool`, then from `bool` to `int`. Then, pick up an ancient operator to finish the line. The result is surprising, and undesirable of course.

C++ has the `explicit` keyword to prevent implicit conversion from happening.
```
class Test {
	int i{42};
public:
	explicit operator int() const { return i;}
}

int main()
{
	Test test;
	cout << test << endl; // this now cannot compile
	cout << static_cast<int>(test) << endl; // have to do an explicit cast now
}
```
However, `explicit` can have no affect if the conversion is to `bool` and the object is used in a conditional check.
```
class Test {
	int i{42};
public:
	explicit operator int() const { return i;}
	explicit operator bool() const { return true; }
}

int main() {
	Test test;
	if (test) { // this scenario will ignore explicit keyword
	...
	}
}
```

There is one more implicit conversion easy to miss, and it is about constructor.
```
class Test {
	int i{42};
public:
	Test(int integer): i(integer) {} // act as conversion operator
	explicit operator int() const { return i;}
	explicit operator bool() const { return true; }
}

int main() {
	Test test = 4;
}
```
The line in `main()`, will try to turn `4` into a Test object by invoking the constructor and put `4` as the argument. Then, it will do a copy constructor. This may not be what you really want. To prevent this, again we can use `explicit` keyword.
```
class Test {
	int i{42};
public:
	explicit Test(int integer): i(integer) {} // act as conversion operator
	explicit operator int() const { return i;}
	explicit operator bool() const { return true; }
}

int main() {
	Test test = 4; // this cannot compile now
	Test test = Test(4); // explicit constructor invocation needed
}
```


