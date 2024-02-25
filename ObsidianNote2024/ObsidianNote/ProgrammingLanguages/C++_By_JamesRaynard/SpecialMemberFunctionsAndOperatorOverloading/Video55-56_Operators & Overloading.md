
An operator which takes a single argument is an unary operator. Usually, it is implemented as a member function.
`c = -b;`

An operator which takes two arguments is a binary operator. Usually, it is implemented as a global function.
`c= a + b;`

An operator which takes three arguments is a ternary operator.
`a ? b : c;`

C++ defines operators for built-in types, and standard library also has done the same for types defined in `std`. However, C++ does not provide operators for the types we defined, except for assignment operator if the compiler synthesizes that.

We can define operators for our types, which is called overloading the operators. But we are limited to use the same operators available for built-in types. The good ones are: `+, -, *, /, =, ==, !=, <, ()`.

There are more: `&&, ||, &, ,, ::, ., *, ?, :`, but these are either not suggested or allowed to be overloaded.

Do not overload operators unless it is unarguably good for your types, because people are most likely need to guess the outcome of an operator, and this uncertainty must be out weighted by the benefits of doing so.


