
Normally, a non-member function can only access a class's public members. However, in the class definition, we can use the `friend` keyword to denote certain functions, and those functions can now access all class's members.

```
class Test {
	int i{42};
	string s{"Hello"};
public:
	friend void print(const Test&); // denote print() as friend
};

void print(const Test& test) {
	cout << "i = " << test.i << endl;
}

int main() {
	Test test;
	print(test);
}
```
Now, the `print()` global function has access to all members of class `Test`.

`friend` can be used to denote another class as well.
```
class Test {
	int i{42};
	string s{"Hello"};
public:
	friend void print(const Test&); // denote print() as friend
};

class Example {
public:
	void print(const Test& test) {
	cout << "i = " << test.i << endl;
	}
};

int main() {
	Test test;
	Example ex;
	ex.print(test);
}
```
Now, inside the class `Example`, we have all access to members of class `Test`.

But why do I want something like this? Can't we just provide an interface for it, instead of letting an outsider to do whatever it wants with all the members?
Well, it has its place when we overload operators.