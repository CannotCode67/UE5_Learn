
Here, we study the most efficient way to pass objects into function arguments. Before C++11, we don't have move semantics, the most efficient way to do that would be passed by const reference. Now with move semantics, does the knowledge still hold true?

We use our custom class `Test` which has a standard string member to be initialized when constructor is called.
```
class Test {
	string m_str;
public:
	//try to find the fastest constructor to initialize m_str
}
```

First, we look at passed by const reference.
```
Test::Test(const string& str): m_str(str) {}
...
string name;
Test test_lvalue(name);
Test test_rvalue(std::move(name));
```
Both `lvalue` and `rvalue` are passed by const reference, and they end up with one copy operation which copies them to become `m_str`.


Second, we look at passed by value, and with this syntax, we get passed by move when the passed object is `rvalue` and moveable.
```
Test::Test(string str): m_str(str) {}
...
string name;
Test test_lvalue(name);
Test test_rvalue(std::move(name));
```
`lvalue` would have two copy operations, one for coping the object into argument object, and one for copying the argument object into `m_str`. `rvalue` would have one move operation for moving the data from `name` into argument object, and one copy operation for copying the argument object into `m_str`.

Thirdly, we look at `rvalue` reference. So no `lvalue` is accepted.
```
Test::Test(string&& str): m_str(std::move(str)) {}
...
string name;
Test test_lvalue(name); // will not compile
Test test_rvalue(std::move(name));
```
`lvalue` would not compile. `rvalue` would have two move operations, one for moving data from `name` into argument object, and one for moving data from argument object into `m_str`.

The conclusion is for a `lvalue`, passed by const reference is still the fastest route with only one copy operation, but for a `rvalue`, the passed by `rvalue` reference is actually the quickest with one move operation. 

And if we can choose between `lvalue` and `rvalue`, then passed by `rvalue` reference is the fastest of all because move operation is much faster than copy operation. But we don't have to choose, we can have both constructors in our example.
```
class Test {
	string m_str;
public:
	//try to find the fastest constructor to initialize m_str
	Test(const string& str): m_str(str) {}
	Test(string&& str): m_str(std::move(str)) {}
}
```
One drawback is we have to write these two overloads ourselves, and that's one argument. With each additional argument we have, this hand written overloads is increased by the multiple of 2. That's why C++11 introduced something called forwarding reference which is specific to solve this problem. And we will study it in the next video.
