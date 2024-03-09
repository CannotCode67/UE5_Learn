
A variadic function can take any number of arguments which have to be of the same type. The arguments are processed at runtime. It is a feature of C rather.

C++11 introduce variadic template functions. A variadic template function can take any number of arguments which can have different types.
The syntax is a little bit weird, and the `...` is known as parameter packs.
```
template <typename... Ts> // Ts is a list of template parameter types
void func(Ts... ts); // ts is a list of arguments whose types match Ts
```

The compiler can deduce the number of arguments and their types from the call.
```
func("hello"s); // instantiated as func(string str);
func(42, 0.0, "text"s); // instantiated as func(int i, double d, string str);
```

The `...` parameter packs are only available at compile time. There are three things we can do with it: use `sizeof...()` to get the number of elements, use `make_tuple()` to store them, and iterate over the elements using template recursion.

To use `sizeof...()` to get the number of elements.
```
template <typename... Ts>
void func(Ts... ts) {
	cout << sizeof...(ts) << endl;
}

int main() {
	int i{42};
	double d{0.0};
	string str{"hello"};
	func(i); // 1
	func(i, d, str); // 3
}
```

To use `make_tuple()` to store them.
```
template <typename... Ts>
void func(Ts... ts) {
	auto ts_tuple = make_tuple(ts...);
	auto first = get<0>(arg_tuple);
	cout << first << endl;
}

int main() {
	int i{42};
	double d{0.0};
	string str{"hello"};
	func(i, d, str); // 42
}
```

To iterate over the elements using template recursion.
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
First, we need to define a non-variadic template function. The variadic template function definition needs to come after that. In our example, the variadic template function will process the first of all arguments and call itself with the rest of arguments. We can separate the arguments by having a single template parameter and a template parameter pack.