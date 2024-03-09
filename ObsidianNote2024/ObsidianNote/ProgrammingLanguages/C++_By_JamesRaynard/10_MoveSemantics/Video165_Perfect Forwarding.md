
Forwarding when involves with function is referring to a function passing some or all of its arguments to another function.
```
void func(Test x) {
	gFunc(x); // func() forwards argument x to gFunc()
}
```

Perfect forwarding means the properties of the passed object are preserved.
If x is modifiable in `func()`, then it is modifiable in `gFunc()`.
If x is unmodifiable in `func()`, then it is unmodifiable in `gFunc()`.
If x is moved into `func()`'s argument, then it will be moved into `gFunc()`'s argument.

Perfect forwarding is used to write functions which call constructors, such as `make_pair()`.

It is also used by variadic templates to dispatch their arguments to functions that process them, whatever that means.

If `gFunc()` has three overloads:
```
void gFunc(Test& x);
void gFunc(const Test& x);
void gFunc(Test&& x);
```
Then, the intuitive approach will be writing three overloads for `func()`.
```
void func(Test& x) {g(x);}
void func(const Test& x) {g(x);}
void func(Test&& x) {g(std::move(x));}
```
Or with forwarding reference we just leant, can we just write a template function instead?
```
template <typename T>
void func(T&& x) {
	gFunc(x);
}

class Test {}

int main() {
	Test x;
	const Test cx;
	func(x); // calls gFunc(Test& x)
	func(cx); // calls gFunc(const Test& x)
	func(std::move(x)); // calls gFunc(Test& x)
}
```
The first two are good, but when we pass the `rvalue` of x into `func()`, we still calls `gFunc()` that handles a `lvalue` reference. Why?

Well, when we pass the `rvalue` of x into `func()`, there is a move operation happening. It moves x's data into the function argument variable. This function argument variable is a `lvalue`. So when it is passed into `gFunc()`, we get the `lvalue` reference version.

Try to solve this with `std::move()`.
```
template <typename T>
void func(T&& x) {
	gFunc(std::move(x));
}

class Test {}

int main() {
	Test x;
	const Test cx;
	func(x); // calls gFunc(Test&& x)
	func(cx); // calls gFunc(const Test& x)
	func(std::move(x)); // calls gFunc(Test&& x)
}
```
Now, the last two are good, but when we pass the `x` as `lvalue` into `func()`, we end up calling `gFunc(Test&& x)`, which is understandable. `std::move()` turns a `lvalue` into a `rvalue`, and I suppose it cannot do it with const.

`std::forward<T>(x)` casts its argument to `rvalue` reference of some type. But it only does that when `x` is of type `T` or `T&&`, otherwise, it will be left as an `lvalue` reference to `T`. This function is perfect for our situation.
```
template <typename T>
void func(T&& x) {
	gFunc(std::forward<T>(x));
}

class Test {}

int main() {
	Test x;
	const Test cx;
	func(x); // calls gFunc(Test& x)
	func(cx); // calls gFunc(const Test& x)
	func(std::move(x)); // calls gFunc(Test&& x)
}
```
It works for the third because the function argument variable is a `lvalue`, not a `lvalue` reference.