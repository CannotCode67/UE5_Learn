
A templated class supports almost all the functionalities of a regular class. Both templated classes and regular classes can have templated member functions but the templated member functions cannot be virtual.

Templated classes can have templated member functions whose template parameters are different from the ones of the classes.
```
template <typename T>
class comparer {
	T t1, t2;
public:
	comparer(const T& t1, const T& t2): t1(t1), t2(t2) {}
	template <typename Func>
	bool compare(Fun f) {return f(t1, t2);}
};

int main() {
	int x{1}, y{2};
	comparer<int> c(x, y);
	auto b = c.compare([](int i1, int i2) {return i1 < i2;});
}
```
The type `Func` must be a callable object which can handle two arguments of `T`, but it may not be clear enough sometimes.

C++20 introduces something called concepts which allow us to express this relationship as part of the template definition.