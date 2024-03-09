
Member function pointer is different from non-member function pointer. Member function pointer cannot be called directly, instead it needs to be explicitly dereferenced before being called. Therefore, it is not a callable object. Non-member function pointer can be called with and without explicitly dereferencing, so it is a callable object.

To define a pointer to a member function of a class. First, we need to have a class with a member function.
```
class Test {
...
void func(int i, const string& str);
};
```
The syntax to declare a member function pointer variable is rather ugly.
`void (Test::*pfunc)(int, const string&);`
Then, we assign the address.
`pfunc = &Test::func;`
To invoke the pointed member function, we need to have an object first. To access the member function, we need to use below syntax.
```
Test test;
(test.*pfunc)(42, "Hello"s);

```
If what we have is a pointer to the object, then we use `->*` instead.
```
Test* pTest = &test;
(pTest->*pfunc)(42, "Hello"s);
```

The process is very similar to non-member function pointer though.
If we have a non-member function like below.
`void globalfunc(int i, const string& str);`
Then, we declare a non-member function pointer variable.
`void (*p_globalfunc)(int, const string&);`
Then, we assign the address.
`p_globalfunc = &globalfunc;`
To invoke the pointed function.
`(*p_globalfunc)(33, "World"s); // explicitly dereferenced`
`(p_globalfunc)(33, "World"s); // implicitly dereferenced`

Just like non-member function pointer, we can use `auto` to save us from writing out the weird type.
`auto pfunc = &Test::func;`
`auto p_globalfunc = &globalfunc;`
Or use a type alias.
`using pfunc = void (Test::*)(int, const string&);`
`using p_globalfunc = void (*)(int, const string&); `

Although member function pointer is not a callable object, it must be something similar based on how close it appears to be compared to non-member function pointer. There are two ways to convert a member function pointer into a callable object.

C++11 provides `std::mem_fn()` under the `<functional>` header. This function takes a member function pointer as argument and returns a callable object.
`auto f = mem_fn(pfunc);`
To invoke this callable object, we need to pass the object to call upon.
`f(test, 42, "Hello"s);`

Another way is to use `std::bind()`. Supposedly we want to leave out the `int` argument.
`auto func_Hello = bind(&Test::func, &test, _1, "Hello"s);`
To invoke it.
`func_Hello(42);