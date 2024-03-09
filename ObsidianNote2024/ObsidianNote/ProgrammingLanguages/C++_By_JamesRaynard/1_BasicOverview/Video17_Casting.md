
A cast performs an explicit conversion. It can be:
1. A conversion for an expression to have a different type.
2. A conversion for an const expression to non-const equivalent.
3. A conversion for data in specific type to be data in untyped binary form.
4. A conversion for a pointer to base class to a pointer to derived class.

The first one is obvious, and we should encounter it often. Like casting a number in string literal into a numeric literal, so that we can do arithmetic calculation. For this one, we use `static_cast<T>(expression)`. 

The second one is about history of C and C++. Const expression was introduced in C++, then imported back into C. Before this happened, code written in C did not care for such a concept. And C++ was designed to compatible with C, resulting C++ had to offer a way to cast the const expression into non-const equivalent to make its compatibility possible. For this one, we use `const_cast<T>(expression)`.

The third one is for working with low-level or transmitting data through internet or serialization, etc. For this one, we use `reinterpret_cast`.

The fourth one is for working with polymorphism. We use `dynamic_cast`. Only this one is done at runtime, others are done at compile time.

C-style casting is something looks like below:
```
int c = 'A';
cout << (char)c << endl; //convert c from int to char
```

