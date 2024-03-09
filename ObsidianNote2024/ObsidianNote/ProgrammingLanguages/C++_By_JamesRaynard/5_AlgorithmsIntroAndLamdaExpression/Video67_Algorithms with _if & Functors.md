
Many algorithms have two versions. The base version takes a value argument. The `_if` version takes a predicate argument. For example, `find()` returns the first element with this value, but `find_if()` returns the first element where the predicate returns true.

We can use a function pointer or a functor to provide this predicate, but now we use functor to see how it works better than function pointer in some cases.

```
class ge_n {
private:
	const int n;
public:
	ge_n(const int n): n(n) {}
	bool operator() (const string& str) const {
		return str.size() > n;
	}
}

int main() {
	vector<string> names = {"Dilbert", "PHB", "Dogbert", "Asok", "Ted"};
	auto res = find_if(cbegin(names), cend(names), ge_n(5));

	if (res != cend(names))
		cout << *res << endl;
}
```
The functor class `ge_n` can take an integer for its constructor. And the integer will be the word length criteria. This give us the flexibility to change the word length criteria when we create the temporary functor object, which is something we cannot have with a function pointer.

Be aware, functor class object is a callable object, so we need to pass an object. That's why we are actually calling the constructor in the place where a callable object is expected.

We can directly uses a functor class without putting it into some functions. We call the constructor to have an object then call its overloaded `()` operator like below.
```
int main() {
	bool result = ge(3)("Hello"s);
}
```