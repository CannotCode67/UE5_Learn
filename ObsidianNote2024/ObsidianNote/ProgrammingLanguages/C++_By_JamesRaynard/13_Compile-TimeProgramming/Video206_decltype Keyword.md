

The `decltype` keyword was added in C++11. It gives the type of its argument which must be an expression or an object. According to cppreference.com, an expression in C++ is a sequence of operators and their operands, that specifies a computation. `decltype` works during compile-time.

Before `decltype`, some compilers have a non-standard extension `typeof`, which is similar to `decltype`.

```
decltype(1+2) y; // replaced as int y;
int x;
decltype(x) z; // replaced as int z;
```
For example above, we can see `decltype` can be used to declare a variable without any initial value. On the contrary, `auto` needs the initial value to deduce the type for the variable. One more difference between `decltype` and `auto` is `decltype` retains qualifiers.
```
const int cx;
auto y = cx; // y's type is int
decltype(cx) z; // z's type is const int
```

`decltype` returns different types based on the passed arguments, but sometimes it returns something you might not expect.

When `decltype` takes a named variable, it returns the type of the variable.
```
int x;
decltype(x) // gives int
```
When `decltype` takes a `lvalue` expression, it returns a `lvalue` reference to the expression's deduced type.
```
int x{0}, y{1};
decltype(x+y) // expression's deduced type is int, so it returns int&
decltype((x)) // (x) is a operator with operand, so a expression, so int&
```

When `decltype` takes a `pure rvalue` which is a literal, the result is the argument's deduced type.
```
decltype(2) // gives int
decltype('c') // gives char
decltype(1.9) // gives double
```
When `decltype` takes a `xvalue` which is temporary object, it returns a `rvalue` reference to the argument's deduced type.
```
decltype(Test()) // gives Test&&
```

In C++14, we can pass `auto` into `decltype` as argument, `decltype(auto)`. This works like `auto`, except it retains qualifiers, so we need the initial value here.
```
const int& a{99};
auto b = a; // int
decltype(auto) c = a; // const int&
```

`decltype` is mainly used in compile-time programming.
Here we have a templated function that add two arguments of any type together. 
```
template <typename T, typename U>
auto add(T t, U u)->decltype(t+u) {
	return t + u;
}
```
Because two arguments can have different types, so the return type of the function is represented by `decltype(t+u)`. We can denote that by putting auto at the place where we normally put a specific type, and use a `->` sign followed with the `decltype` representation. The way this works is if we have an `int` and a `float`, then `decltype` will return `float`. But you cannot expect it can work out with about any combination of types.

It can also be used at places where a template parameter is expected, but you don't know the type because the type is deduced automatically by the compiler using `auto`.
```
auto end_connection = [](connection *conn){ToDisconnect(*conn)};`

void Get_Data(const destination& dest) {
	connection conn = connect(dest);
	unique_ptr<connection, decltype(end_connection) ptr(&conn, end_connection);
}

auto make_vector = [](auto x, size_t n) {
	return vector<decltype(x)>(n, x);
}
```