
An object can either be a `lvalue` or a `rvalue`. Originally, in the days of ancestors of C, the rule to tell whether an object is a `lvalue` or a `rvalue` is simple. A `rvalue` can only appear on the right side, and vice versa.
` x = 2; // x is a lvalue, 2 is a rvalue`

But this rule doesn't work with C++ as we have operators return things which can be assigned to. The rule for C++ is:
A `lvalue` represents a named memory location, so it has a name and we can take its address using `&` operator.
`x; // name is x, and &x is legal`
A `rvalue` is anything else.
`2; // 2 has no name, &2 is not legal`
`func(); // func() return value has no name, and &func() is not legal`
The reasoning behind is if we want to take the address of an object, we would like to change its value in the memory. But as you can see, it makes no sense to change a constant number or a return value, thus it is not allowed.

Why bother with all these `lvalue` and `rvalue` things? Well, they can behave differently when passed into functions.
`lvalue` and `rvalue` can both be passed by value.
`lvalue` can be passed by address, but not `rvalue` since we cannot take the address from it.
One exception where `rvalue` can be passed by address, is passed by const reference.

Now, with move semantics, the argument can be moved if it is a `rvalue`, and its type is moveable. All standard library types are moveable except for the ones of multi-threading. And the argument will be copied if it is a `lvalue` or its type is not moveable. There is no syntax for it, we just write function call the way before, which enables the compatibility with old code and let the move semantics to work behind the scene.

Being movable means the types have its move constructor and move assignment operator defined. All built-in types in C++ are moveable.
