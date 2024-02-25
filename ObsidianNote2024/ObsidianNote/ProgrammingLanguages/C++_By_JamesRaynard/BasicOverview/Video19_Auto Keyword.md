
The keyword `auto` was originally used in C to specify a variable whose memory is allocated on the stack. Because variable lives on the stack gets destroyed automatically, hence the meaning of auto. But that is very cumbersome, and the keyword was left behind.

C++ recycle the keyword `auto`, but here it is used to indicate the compiler should deduce the type from the variable's initial value. For the line of `auto i{46};`, `i` will have a type of `int`, determined by the compiler.

A catch for `auto` is it ignores const, reference, etc.
```
const int& x{6};
auto y = x; // y would have a type of int
++y; // this line works
```
If we need to add those qualifiers, we can do this `const auto& y = x;`.