
 We all know number in string literal and number in numeric literal are two different things as they have different types. To convert them, C has functions like `atoi()` for converting C-Style string to `int`, and `atod()` for converting C-Style string to `double`. For the other way around, C has `sprintf()`. These are all inherited by C++.

For standard string, we have different functions of course.
`to_string()` takes in all integer and floating-point types, and converts its argument into a `std::string`.
`cout << "Hi "s + to_string(3.14159) << endl; // displays "Hi 3.14159"`

`stoi()` takes a `std::string` and turns it into an `int`.
```
string str{"42"};
cout << stoi(str) << endl; // displays 42
str = {" 314 159"};
cout << stoi(str) << endl; // display 314
```
From the above example, we can see `stoi()` ignores leading whitespace, and if it finds something other than a number in the middle, the conversion stops. As result, we only have 314.

To handle weird results, `stoi()` can accept second argument which is a type of `size_t` in reference. If `stoi()` completely succeeds, the second argument would be equal to the string's size. If it is partially successful, the second argument would be the index pointing to the first character fails to be converted. If it fails completely, we get an exception.
```
size_t n_processed;
auto i = stoi(str, &n_processed);
if (n_processed < str.size())
{...}
stoi("abcde"); // throws an exception
```

`stoi()` takes an third argument to decide the numeric base. By default, it is decimal.
`cout << stoi("2a", nullptr, 16); // display 42`
And if we don't want to use its error handling second argument, we can just give it a `nullptr`.

`stod()` takes a `std::string` and turns it into an `double`, but it has to use decimal.