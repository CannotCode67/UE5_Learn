
Before move semantics, we have four special member functions for a class, the constructor, the copy constructor, the assignment operator, and the destructor. But as move semantics was introduced, people find out that overloading the copy constructor and assignment operator, so that they take `rvalue` reference, is actually very useful. Because copy constructor and assignment operator are often involved with a lot of copying, and if we can use move semantics instead, then we should get a good performance boost. So, now we have six special member functions for a class, the constructor, the copy constructor, the move constructor, the assignment operator renamed as the copy assignment operator, the move assignment operator, and the destructor.

Here are the signatures of them.
```
Test(const Test& arg); // copy constructor
Test(Test&& arg) noexcept; // move constructor
Test& operator= (const Test& arg); // copy assignment operator
Test& operator= (Test&& arg) noexcept; // move assignment operator
```
We see the move constructor and move assignment operator don't take in argument const, because we will move the data out of the argument object, so there will be modification. Both move member functions are marked with `noexcept`. The reason is there is no simple way to recover from a half-way move operation, therefore we should never use any code that might throw an exception in move operations. Plus, standard library containers only call an element's move operator if it is `noexcept`.

We now see an example where we invoke the move operations of the member by passing in a `rvalue`.
```
class Test {
private:
	int i{0}; // built-in member
	MyClass m; // custom member
public:
	Test() = default;
	Test(const Test& arg): i(arg.i), m(arg.m) {}
	Test(Test&& arg): i(arg.i), m(std::move(arg.m)) {}
	Test& operator= (const Test& arg) {
		if (this != &arg) {
			i = arg.i;
			m = arg.m;
		}
		return *this;
	}
	Test& operator= (Test&& arg) {
		if (this != &arg) {
			i = arg.i;
			m = std::move(arg.m);
		}
		return *this;
	}
};

int main() {
	Test test; // default constructor
	Test test2 = test; // copy constructor
	Test test3 = Test(); // move constructor
	Test test4(std::move(test)) // move constructor
	Test test5;
	test5 = test2; // copy assignment operator
	Test test6;
	test6 = Test(); // move assignment operator
}
```

When writing a move operator for a derived class, we should call the corresponding operator for the base class. To do this, we must pass the argument as an `rvalue`. Otherwise, we would be writing the copy version.
```
// copy constructor
Derived(const Derived& arg): Base(arg) {...}
// move constructor
Derived(Derived&& arg): Base(std::move(arg)) noexcept {...}
```
