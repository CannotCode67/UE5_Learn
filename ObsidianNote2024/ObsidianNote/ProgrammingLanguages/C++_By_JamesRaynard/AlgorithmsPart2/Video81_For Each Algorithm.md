
`std::for_each()` calls a function on every element in an iterator range.
```
string str{"Hello World};
for_each(cbegin(str), cend(str), [](const char c) {cout << c << ",";});
for_each(begin(str), end(str), [](char& c){c = toupper(c);});
```

`std::for_each()` came in C++98, and range-for loop came in C++11. Nowadays, if possible, we should use range-for loop over `for_each()`. But, `for_each()` was rewritten to be compatible with parallel processing in C++17.