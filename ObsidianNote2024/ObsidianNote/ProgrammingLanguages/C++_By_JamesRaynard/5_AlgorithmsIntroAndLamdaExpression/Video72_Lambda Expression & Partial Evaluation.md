
Lambada expressions are first class object just like function pointer, meaning we can store the lambda expression and pass it to functions, or functions can return lambda expression.

To store a lambda expression in a variable:
`auto is_longer = [max](const string& str) {return str.size() > max;}`
Then pass the variable to a function.
`auto res = find_if(cbegin(words), cend(words), is_longer);`

Function to return a lambda expression:
```
auto greeter(const string& salutation) {
	return [salutation](const string& name) {return salutation + ", "s + name;};
}
```
Calling this function will return a lambda which captures `salutation`.
`auto greet = greeter("Hello"s);`
Now `greet` is a lambda which takes a name as argument and adds the salutation to it.
`greet("student"s);` will return a string of `Hello, student`.

This feature is called partial evaluation where data is processed in stages. The first part we give it the value to capture, the second part we give it the actual parameter value.

Our example, we capture `salutation` by value. If we capture it by reference, then when we give it the actual parameter value, we have to make sure the captured variable is still valid. Otherwise, it will crash the program.