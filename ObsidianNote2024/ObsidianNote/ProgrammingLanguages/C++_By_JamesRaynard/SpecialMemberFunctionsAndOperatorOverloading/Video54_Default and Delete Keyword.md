
We know compiler would synthesizes special member functions if we don't write them. However, sometimes we expect the compiler to synthesize one for us, but it doesn't. Like we write a copy constructor, and expect a default constructor synthesized by compiler. But the compiler would not provide one as copy constructor is a constructor after all. Therefore, to make this behavior very clear, we have the `default` keyword to force the compiler to give us the default special member functions.
```
class Test {
public:
	Test() = default;
	Test(const Test&) = default;
	Test& operator=(const Test&) = default;
}
```
This is recommended. It avoids the confusion whether the programmers forget to implement them or want them to be provided by compiler.

In older versions of C++, if we want to prevent our objects from being copied, we can put the copy constructor or assignment operator into private section. Now, we have the `delete` keyword.
```
class Test {
public:
	Test() = default;
	Test(const Test&) = delete;
	Test& operator=(const Test&) = delete;
}

int main()
{
	Test test1, test2;
	Test test3(test1); // this cannot compile now
	test2 = test1; // this cannot compile now
}
```
The `delete` keyword actually works for all functions, including global ones. And even destructor can be deleted, but this should never be done.

Another situation can happen where functions are implicitly deleted. When we use the `default` keyword to force compiler to synthesize a default constructor, it would fail if a member of the class cannot be default initialized. If this happens, compiler would still give us a default constructor but it is deleted one.