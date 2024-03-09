
Recall the example when we study variadic templates.

We use variadic template to iterate over the elements using template recursion.
```
template <typename T>
void func(T t) {
	cout << t << " is the end" << endl;
}

template <typename T, typename... Ts>
void func(T t, Ts... ts) {
	cout << t << endl;
	func(ts...);
}

int main() {
	int i{42};
	double d{0.0};
	string str{"hello"};
	func(i, d, str);
}
```
Here we need to write two templated functions, one with a function call, and one without. With `constexpr` if statement, we can write only one template function to achieve the same result.
```
template <typename T, typename... Ts>
void func(T t, Ts... ts) {
	cout << t << endl;
	if constexpr (sizeof...(ts) > 1)
		func(ts...);
	else
		cout << ts << " is the end" << endl;
}

int main() {
	int i{42};
	double d{0.0};
	string str{"hello"};
	func(i, d, str);
}
```


Another `constexpr` if example is the Fibonacci number function.
```
int fibonacci(int n) {
	if (n > 1) {
		return fibonacci(n-1) + fibonacci(n-2);
	}
	return n;
}
```
It is easy to write when the function is executed at runtime. But if we want it to be executed at compile-time, we have to write a couple of specializations.
```
template <int N>
int fibonacci() {
	return fibonacci<N-1>() + fibonacci<N-2>();
}

template<>
int fibonacci<1>() {
	return 1;
}

template<>
int fibonacci<0>() {
	return 0;
}

int main() {
	constexpr int n{10};
	cout << fibonacci<n>() << endl;
}
```
This example shows how template parameters are used to represent state variables, so that the whole thing can be executed at compile-time.

With `constexpr` if statement, this is a much simpler code to write.
```
template <int N>
constexpr int fibonacci() {
	if constexpr (N > 1) {
		return fibonacci<N-1>() + fibonacci<N-2>();
	}
	return N;
}

int main() {
	constexpr int n{10};
	cout << fibonacci<n>() << endl;
}
```
