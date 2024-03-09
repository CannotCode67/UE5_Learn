
To make a move-only class, all we need to do is making sure there is no copy constructor and copy assignment operator for the class. So we ask the compiler not to synthesize any for us.
```
class Test {
publuc:
	Test(const Test&) = delete;
	Test& operator= (const Test&) = delete;
	...
}
```
We still have the constructor, the move constructor, the move assignment operator, and the destructor.

But why would you want such a type? Well, if this class manages resource of some kind, then being impossible to be copied perfectly fits the rule where only one object should own a given resource at a time. The C++ `fstream`, `iostream` and smart pointer classes are all examples of move-only class.

There was a problem working with move-only class using lambda expression. As we know, lambda needs to capture variables if it needs access to local variables. We know it can capture by value, or by reference, but there wasn't any way to capture by move at least until C++14. After C++14, lambda express can create and initialize its own scope local variable. And this with `std::move()` can achieve capture by move.
```
int main() {
	vector<string> strings(5);
	[&strings](){}(); // the () in the end is necessary to call a lambda this way
	strings.size(); // 5
	[vs=std::move(strings)](){}(); // after this, strings is empty
	strings.size(); // 0
}
[lfs = std::move(fs)](){...}
```
The difference between capture by reference and by move is by reference, we are dealing with the local variable directly, and if we do nothing in the lambda, then nothing will happen to the local variable. By move, we transfer all the data members into the variable local to lambda scope, even if we do nothing, the local variable will lose all its data member.