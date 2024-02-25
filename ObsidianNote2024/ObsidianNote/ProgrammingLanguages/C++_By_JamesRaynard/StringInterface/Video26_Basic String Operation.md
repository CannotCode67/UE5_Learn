
The standard string has a history before it became part of the standard library. Originally, the `string` class was designed to be a standalone class, specific to deal with words. Later, designers for standard library think it would be nice to make `string` class interface to be consistent with other standard containers. The result is `string` class interface is big, with over 100 member functions.

Below are some basic string operations of standard string.
Assignment, `string1` will have the value of `string2`. `string2` can be a C-Style string and it still works.
`string1 = string2;`

Appending, the value `string2` will be added at the back of `string1`. `string2` can be a C-Style string or char.
`string1 += string2;`

Concatenation, returns a new string with the value of `string1` followed by the value of `string2`. If either `string1` or `string2` is a C-Style string or char, it would still work.
`string1 + string2;`

Comparison, returns a Boolean based on the comparison operator: `==`, `!=`, `>`, `<`, `<=`, `>=`. If `string2` is a C-Style string or char, it will still work.
`string1 cmp string2; // cmp being one of the comparison operator`

`std::string` has a member function `c_str()`which generates a copy of the string's value in the form of C-Style string, and returns a pointer to that copy.
`const char* pChar = str.c_str();`
It is useful when we need to deal with old code or low-level code written in C.

Another member function `substr()` returns a substring based on the arguments.
```
string str{"Hello You"};
substr1 = str.substr(6); // returns "You"
substr2 = str.substr(6, 2); // returns "Yo"
```

Various forms of constructor for `std::string`
```
string string1; // string1 is without value
string string2{"Hello"}; // the usual one
string string3(3, 'x'); // string3 has a value of "xxx"
string string4{'H', 'e', 'l', 'l', 'o'}; // the second one is preferred
string string5(string2, 1); // "ello"
string string6(string2, 1, 3); // "ell"
```