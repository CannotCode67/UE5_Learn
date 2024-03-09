
`constexpr` if statement was added in C++17, which is also known as if `constexpr` or static if. It evaluates the conditionals at compile-time. The regular if statement checks the conditionals at runtime.

The syntax is straightforward.
```
if constexpr (a < b) {
	...
}
else {
	...
}
```
It is used for conditional compilation. Only the codes in one of the branch will be compiled into the program. It is how metaprogramming achieves control flow.

Let's see an example where a templated function accepts an argument and return a standard string representing that argument.
```
template <typename T>
string get_string(const T& arg) {
	if (std::is_same<std::string, T>::value)
		return arg;
	else
		return to_string(arg);
}

int main() {
	int x{42};
	string str{"Hello"};
	get_string(x);
	get_string(str);
}
```
Unfortunately, this will not compile. As we use `if` instead of `if constexpr`, the conditional is evaluated at runtime, meaning the compiler doesn't know which branch will be taken, so it needs to make sure all the branches must compile. 

So if we have argument as `std::string`, in the else branch, the code goes like `to_string(string)`, which cannot compile because `to_string()` has no overloads to accept a `std::string` argument.

The conditional is checked and replaced by the result at compile-time. `std::is_same<std::string, T>::value` is replaced by `true` when compiler sees `get_string(str)` and instantiates the function. So during compile-time, the instantiated code looks like below. 
```
string get_string(const string& arg) {
	if (true)
		return arg;
	else
		return to_string(arg);
}
```
The else part will never be taken. The information is enough to deduce that. We, the programmers, can naturally do that, but compiler cannot with `if` statement.

That's why we need `constexpr` if statement, to let the compiler check the conditional and make the decision at compile time. Once the compiler checks the conditional is true, it will ignore the else part without reading it.
```
template <typename T>
string get_string(const T& arg) {
	if constexpr (std::is_same<std::string, T>::value)
		return arg;
	else
		return to_string(arg);
}
```

Before `constexpr` if statement, C++ has pre-processor directives which can be used for conditional compilation. `#if` and `#ifdef` conditionally include or exclude code from compilation.

Before C++17, a solution to the same problem is template specialization.
```
template <typename T>
string get_string(const T& arg) {
	return to_string(arg);
}

template<>
string get_string(const string& arg) {
	return arg;
}
```

Another solution would be substitution failure is not an error, short for SFINAE with `std::enable_if`.
```
template <typename T, std::enable_if_t<!std::is_same_v<std::string, T>, int> = 99>
string get_string(const T& arg) {
	return to_string(arg);
}

template <typename T, std::enable_if_t<std::is_same_v<std::string, T>, int> = 99>
string get_string(const T& arg) {
	return arg;
}
```
Here, we basically have two templated functions with the same name. How does the compiler decide to use which one? Well, the SFINAE trick is making one of the templated function disabled, so the compiler will have no choice but to use the only one still enabled. The syntax is ugly, but we can roughly tell that when `T` is equal to `string`, it returns some kind of integer value to compare with 99, and that whole thing becomes something that can enable or disable the templated function.

Those are what we have to solve the problem before `constexpr` if statement.
