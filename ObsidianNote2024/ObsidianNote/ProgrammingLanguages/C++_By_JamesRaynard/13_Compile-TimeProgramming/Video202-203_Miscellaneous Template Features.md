
C++ inherit `assert()` from C, and it is under `<cassert>` header. This checks a condition at runtime. If the condition is true, the program continues to execute. If false, it calls `std::abort()` to terminate the program. It can be disabled by `#define NDEBUG` directives
```
//#define NDEBUG

int main() {
	int x{24};
	assert(x == 42); // terminate the program
	x = 42;
}
```

C++11 introduced `static_assert()` which takes a constant bool expression and a string literal. The bool expression is checked at compile-time. If false, the compilation stops, and the string literal will be used as part of the compiler's error message.
```
static_assert(sizeof(int*) == 8, "require a 64-bit computer");
```
It is mainly used in template metaprogramming.
`static_assert(std::is_copy_constructible<T>::value, "Argument must be copyable");`

We can write a template class which has default parameters.
```
template <typename T = int>
class number {
	T value;
	...
}

int main() {
	number<double> double_num{1.9}; // double_num is 1.9
	number<> default_num(1.9); // default_num is rounded as 1
}
```
When we do not provide the template parameter, the default parameter will be used.

From C++11, we can also write a template function with default parameters.
```
template <typename T = int>
void func(const T& t1, const T& t2) {
	...
}
```
But doesn't template function always get its template parameter from its passed arguments? What is the point to set a default value?

Well, C++11 defines some generic operator classes under `<functional>` header. They are functors and their overloaded `()` operator just calls the corresponding operator of the type.
For example, we have `less<T>(arg1, arg2)` which compares two arguments and returns the result as a Boolean. Its overloaded `()` operator just calls the `<` operator of type `T`.

Now, why to allow template function to have a default value?
```
template <typename T, typename Func = less<T>>
bool compare(const T& t1, const T& t2, Func func = Func()) {
	return func(t1, t2);
}
```
Now, if we don't provide this callable object, the function `compare` would have a default callable object of `less<T>()`. And that's how some functions in the standard library to have a default predicate.