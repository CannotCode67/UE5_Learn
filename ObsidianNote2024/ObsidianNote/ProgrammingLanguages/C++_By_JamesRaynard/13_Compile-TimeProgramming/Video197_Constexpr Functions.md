
The `constexpr` keyword can mark not just variables, but also functions. `constexpr` functions take constant expressions for arguments and return a constant expression. They are executed at compile time by a compile-time interpreter which is part of the compiler and capable of understanding most of C++. That's why we can write `constexpr` functions in normal C++ code.

We put the `constexpr` keyword before the function.
`constexpr double miles_to_km(double miles){return miles * 1.602;}`

A `constexpr` function must be pure, meaning it cannot modify its argument or any other global/static variables.

`constexpr` functions are implicitly defined as inline, which makes it possible to have multiple definitions as long as they are in different headers.

`constexpr` functions can be called with arguments that are not constant expressions. When this happens, the function is treated as regular function. Its return value will not be a constant expression, and it is evaluated at runtime. The reason for this design is we don't need to write two identical functions.
```
constexpr double miles_to_km(double miles) {return miles * 1.602;}
const double dist1 = miles_to_km(40); // evaluated at compile time
double arg{40};
double dist2 = miles_to_km(arg); // evaluated at runtime
constexpr dist3 = miles_to_km(arg); // will not compile
int main(){}
```

In C++11, the body of a `constexpr` function could only contain a single return statement. In C++14, it can have multiple statements, variable declarations and flow control. But code that will cause an runtime operation is not allowed, such as `new`/`delete`, calling virtual function, or throwing exception, etc.

A member function can be marked as `constexpr`. It takes constant expression arguments and returns a constant expression. `const` keyword describes a member function which cannot modify `this` object. In C++11. a `constexpr` member function were also `const` automatically. But in C++14, a `constexpr` member function are not marked as `const` unless we do.

Classes and structs can have members which are `constexpr`. They must be initialized from constant expression and cannot be modified. Also, they must be static because static members are there without any object creation, so no requirement for runtime operation.