
`std::string` has a member function `find()` which looks for the first occurrence of its argument and returns the index. `find()` accepts char, C-Style string, and standard string as arguments. It is case sensitive.
```
string str{"Hello World"};
str.find('o'); // returns 4
str.find("or"); // returns 7
str.find("D"); // returns string::npos
```
If `find()` fails to find a match, it returns `string::npos`, a special value representing an impossible index. It is always good to check if the returned index is equal to this `string::npos`, before using it.

`rfind()` member function is like `find()` but starts its search from the end to the front.
```
string str{"Hello World"};
str.rfind('o'); // returns 7
str.rfind("or"); // returns 7
str.rfind('o', 5); // returns 4, search backwards starting from element with index5
```
The returned index is still counting from the front.

`find_first_of()` and `find_last_of()` search for the first or last occurrence of any character from the argument string. 
`find_first_not_of()` and `find_last_not_of()` search for the first or last occurrence of any character NOT from the argument string.
```
string str{"Hello World"};
string vowels{"aeiou"};
str.find_first_of(vowels); // returns 1
str.find_last_of(vowels); // returns 7, searching backwards
str.find_first_not_of(vowels); // returns 0
str.find_last_not_of(vowels); // returns 10
```