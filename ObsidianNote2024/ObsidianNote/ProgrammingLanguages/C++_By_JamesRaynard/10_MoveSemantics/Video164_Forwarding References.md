

Before we study forwarding references, we need to understand some basics first.

Nested references are references that point to other references. But C++ doesn't provide a syntax to directly do that.
```
int i{2};
int& ref_int = i;
int&& ref_ref_int = ref_int; // this will not compile
```
It wasn't because `&&` is used for `rvalue` reference. It was always like that before C++11. The workaround for that is to use type alias or a template parameter.
```
using ref_int = int&;
// typedef int& ref_int;

int i{2};
ref_int ref_i = i;
ref_int& ref_ref_i = ref_i; // this now compile
```
The compiler will perform something called reference collapsing which turn `ref_int&` from reference to reference to int into reference to int. The rule was easy to understand, but that was the good old days of C++98 with only `lvalue` reference to worry about.

Now with `rvalue` reference, the rule becomes complicated.
```
using lvalue_ref = int&;
using rvalue_ref = int&&;
```
The rule goes like this:
`lvalue_ref&` will collapse into `int&`.
`lvalue_ref&&` will collapse into `int&`.
`rvalue_ref&` will collapse into `int&`.
`rvalue_ref&&` will collapse into `int&&`.
Only a `rvalue&&` to another `rvalue&&` will collapse into `rvalue&&`. If we have a `lvalue` reference anywhere, it will collapse into a `lvalue` reference.

Now, consider the `rvalue` reference we study before, we said that a function takes `rvalue` reference then its argument must be a `rvalue` and also moveable.
`func(Test&& t); // t is a rvalue reference of Test`
If we use replace the specific type `Test` with template argument parameter, we get this.
```
template<typename T>
func(T&& x);
```
`x` is now a `rvalue` reference of type `T`. However, the `&&` here also describes x as a forward reference, meaning the argument type will be deduced by the compiler based on not just x's type but also its value category. So `x` can be both `lvalue` and `rvalue`.

The rule is:
If the passed object is a `lvalue` of type `Test`, then T is deduced as `lvalue` reference to `Test`. The argument type `x` now changes from `rvalue` reference to `T` into `rvalue` reference to `Test&`. Then reference collapsing happens, it will collapse into `Test&` according to the rule.

If the passed object is a `rvalue` of type `Test`, then T is deduced as `Test`. The argument type `x` now changes from `rvalue` reference to `T` into `rvalue` reference to `Test`. There is no need for reference collapsing to happen.

```
class Test{}; // compiler will synthesize the move operations for us

template <typename T>
void func(T&& x) {}

int main() {
	Test t;
	Test& ref_t = t;
	const Test const_t;
	// T is Test&, x changes into rval ref to Test&, collapses into Test&
	func(t); // func(Test& x)
	// T is Test&, x changes into rval ref to Test&, collapses into Test&
	func(rt); // func(Test& x)
	// T is Test, x changes into rval ref to Test
	func(std::move(t)); // func(Test&& x)
	
	func(const_t); // func(const Test& x)
}
```
So, that's one template function which can generate overloads to handle both the `rvalue` reference and `lvalue` reference. This is how forward references solve the drawback we find out at the end of last video. If we want the `lvalue` reference to be const, then we need to pass a const `lvalue` object.