
According to cppreference.com, an expression in C++ is a sequence of operators and their operands, that specifies a computation. 

A constant expression has a value evaluated at compile time and that value cannot be changed. Examples are literals, values computed from literals, values computed from other constant expressions.

A variable can be constant expression if it is initialized by a constant expression and it cannot subsequently be modified.
`const int i{42}, j{99}; // both i and j are constant expressiones`
`i+j; // value computed from constant expressions is constant expression`

One subtlety of C++ is the C-Style array dimension must be a constant expression.
```
const int i{42}, j{10};
int arr[i+j]; // will compile
int k{22};
int arr1[k]; // will not compile
```

We have `const` keyword in C++ to describe a variable. This variable cannot be modified after it has been initialized, and it has to be initialized upon declaration. We can only initialize a const variable from a constant expression.

C++11 introduced the `constexpr` keyword to indicate a variable is a constant expression. A `constexpr` variable is evaluated at compile time and cannot be modified.
