
C++11 introduced lambda expression. It is treated as anonymous local functions. When the compiler sees a lambda expression, it will generate codes that define a functor class which has a name chosen by the compiler and an overloaded `()` operator doing the same thing as the lambda expression indicates.

It is because lambda expression underneath is implemented as functor, that's why we study functor first before going into lambda expression.

The lambda expression syntax is below:
`[] (int n) { return (n % 2 == 1);}`

A full example below:
`auto odd_it = find_if(cbegin(vec), cend(vec), [] (int n) { return (n % 2 == 1);});`

Here, the compiler can deduce the return type is bool, but we can specify a return type explicitly, by using the `->` operator.
`[] (int n) -> bool { return (n % 2 == 1);}`

When the body of lambda expression has branches, C++11 needs us to explicitly write out the return type like above. In C++14, the compiler can handle it given all branches return the same type. In C++17, the compiler can handle branches body even different branches return different types.