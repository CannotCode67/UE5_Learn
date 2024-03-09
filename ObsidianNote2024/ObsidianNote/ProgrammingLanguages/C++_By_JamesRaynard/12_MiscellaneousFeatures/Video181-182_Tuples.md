
C++11 provides `std::tuple` under the `<tuple>` header. It is a templated type similar to `std::pair`. The difference is `pair` takes exactly two elements, while `tuple` can take any fixed number of elements.

Usually, `tuple` is useful as temporary container to store data in different types. It is easier than writing a struct. If you have many values to return from a function, then we can use `tuple` as well.

Here we have a `tuple` take three elements.
`tuple<double, int, string> numbers(1.0, 2, "three"s);`

There is also a function `make_tuple()` to create a `tuple` object.
`auto numbers {make_tuple(1.0, 2, "three"s)};`

To access a `pair` object's elements, we have `first` and `second`. But with `tuple`, we need to use the global function `std::get<>()`. Its template parameter is the index to access.
`auto x = get<0>(numbers); // x is 1.0`

It can be put in the left side to work as value assignment.
`get<1>(numbers) = 3; // numbers{1.0, 3, "three"s)`

In C++14, we can even pass the type as the template parameter.
`auto i = get<int>(numbers); // i is 3`
This requires the type must be unique in the tuple object.

To get all elements in a `tuple`, we use `std::tie()` function. It accepts three variables, and those variables must have the corresponding types of the `tuple` object. Also, they need to be passed in the order which the `tuple` object's elements are put under. After `tie()`, those variables will have the values equal to elements in the `tuple` object.
```
double d;
int i;
string str;
tie(d, i, str) = numbers;
```

C++17 offers some features which happen to make it easier to work with `tuple`.
The constructor template argument deduction, short for CTAD, can deduce the types with initial values.
```
tuple<int, double, string> tup{1. 2.0, "three"s};
tuple tup{1. 2.0, "three"s}; // C++17 with CTAD
```

Structure binding can be used to unpack tuples.
```
tuple<double, int, string> func() {
	return {1.0, 2, "three"s};
}

auto [d, i, str] = func(); // no need to use tie()
```

C++ has a function `std::apply()` which takes a callable object as first argument, and the second argument is a `tuple` containing the arguments to be passed to that callable object.
```
void func(int i, double d, string s);
apply(&func, tuple(1, 2.0, "three"s));
```

We can also use `tuple` to pass values into a constructor with `std::make_from_tuple<T>()`. The template parameter is the class, the argument is the actual `tuple` object.
```
class Test {
private:
	int i;
	double d;
	string str;
public:
	Test(int i, double d, string str): i(i), d(d), str(str){}
}

int main() {
	tuple<int, double, string> tup{1, 2.0, "three"s};
	Test test = make_from_tuple<Test>(tup);
}
```