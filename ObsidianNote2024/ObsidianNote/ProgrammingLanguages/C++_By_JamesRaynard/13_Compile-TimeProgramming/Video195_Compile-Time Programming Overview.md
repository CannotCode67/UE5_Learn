
Compile-time programming means the code is executed by the compiler at compile-time. The result of the execution will be available in the program at runtime. So we pay nothing to get this part of the code executed, which is desirable.

The compile-time programming can be done in C already, by using the pre-processor macro functions. The first stage of the compilation is pre-processor, where compiler goes through the code and removes comments, compresses whitespace and so on. Pre-processor directives start with `#` sign.
One of the pre-processor directives is to define a symbol. Here we define a symbol `Max(x, y)` with a definition of `((x)>(y)?(x):(y))`.
```
#define Max(x, y) ((x)>(y)?(x):(y))
```
We can use this symbol in our code. 
```
int main() {
	int a{5}, b{2};
	cout << Max(a, b) << endl; // will be replaced by ((a)>(b)?(a):(b))
}
```
However, it is not easy to use. First of all, we have no type safety. The pre-processor stage is text processing, so the compiler by this point has no idea of any type. Also this text processing may not be as intelligent as you expect.
```
Max(++a, b); // replaced by ((++a)>(b)?(++a):(b))
cout << a << endl; // a is 7, incremented twice
```
In the example above, we expect an order to do things, so `a` gets incremented first, then put into the `Max()` macro function. However, the text processing is just not that intelligent.

C++ has templates, and they were introduced to support generic programming in the first place. But later it is discovered to be a Turing-complete programming language on its own.
Template parameters can represent state variables.
Recursive instantiation can simulate loops.
Template specializations to implement control flow.
Integer operations to calculate results.

If we use templates to do compile-time programming, then it is known as template metaprogramming.

C++11 introduces a bunch of templated classes called type traits under the `<type_traits>` header. They are used to get the information of a type's properties.

For example, `is_arithmetic<T>` will store a Boolean based on whether `T` is an integer. We can access this value under its data member `value`.
```
class A {};

int main() {
	std::is_arithmetic<int>::value; // we put int there, so we get true.
	std::is_floating_point<int>::value; // false;
	std::is_class<A>::value; // true
	std::is_pointer<const char*>::value; // true
}
```

Before C++11, we only have pre-processor macro functions and templates as tools for compile-time programming. They are hard to write, hard to use, and hard to debug. A debugger requires a running program in memory, so it doesn't work with compile-time programming.

C++11 provides `constexpr`, short for constant expression, which allow us to write things that look like normal C++ code, but are executed by the compiler at compile time rather than runtime. And if we make a mistake, we get error messages like the ones we get with normal C++ code.