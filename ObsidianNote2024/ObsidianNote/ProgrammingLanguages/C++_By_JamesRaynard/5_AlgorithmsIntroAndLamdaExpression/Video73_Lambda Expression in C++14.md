
When we use lambda expression, we need to provide the parameter type. However, after C++14, we can put `auto` there to ask the compiler to deduce the type for us.
```
auto func = [](auto x, auto y) { return x + y;};

func(2, 5);
func(3.141, 4.2);
func(str1, str2);
```
Now, we have to write the lambda expression once, and it can handle various types. It is called generic lambda.

Another feature for lambda is to create variable local to the lambda expression.
`[y=2](int x) {return x + y;};`
We have to initialize this local variable because its type is deduced by the compiler.
We can also initialize it with captured variable, which allows the compiler to do capture by move.
```
int z{1};
[y=z+1](int x) {return x + y;};
```