
Copy elision is a compiler optimization technique where the compiler is allowed to skip over a call to the copy constructor in some cases. The benefit is huge since with a call of constructor (copy constructor), there must be a call of destructor. Those two special member functions are relatively resource-heavy. Most modern compilers have this feature turned on by default, and some of them provide options for programmers to turn it off when compiling. Why do we want to turn this off? Well, this copy elision technique operates regardless the copy constructor is written by us or by the compiler. So, if our copy constructor has some side affects such as `cout << "copy constructor invoked!" << endl;`, those side affects will be skipped altogether.

Now, what cases will cause the compiler to utilize this technique?
When there is a temporary object involved with copying.
```
class Test{
	public:
		Test() { cout << "default constructor invoked!\n"; }
		Test(const Test& other) { cout << "copy constructor invoked!\n"; }
};

Test func() {
	return Test();
}

int main() {
	Test test = func();
}
```
This example has two places of temporary object copying. So with copy elision, we don't see any `"copy constructor invoked!\n"` output in the console.
The first place is `func()` function returns by value, so the test object created by `Test()` will be copied into the function's return space. Copy elision happens here, so the test object created by `Test()` is directly assigned to `func`'s return space in order to avoid copy constructor. This is known as return value optimization.
The second place is `Test test = func();`, where `func`'s return space object is copied to create the test object in `main()`. Copy elision happens here as well, so the test object lives in `func`'s return space is assigned to the `test` variable now without explicitly creating a new one.

Another similar situation is returning a local variable in a function.
```
Test func2() {
	Test test;
	return test;
}
```
This looks similar but compilers may not apply copy elision here. This is known as named return value optimization, which the compiler needs to do extra work to deduce if the returned object is a temporary object or not. So some compilers just don't do the optimization here at all.

The example above is around function, but any temporary object scenario can cause copy elision to take place.
```
string temp{"Hello"};
string word{temp};
// and if temp is never used afterwards, then this will be optimized to below
// string word{"Hello"};
```