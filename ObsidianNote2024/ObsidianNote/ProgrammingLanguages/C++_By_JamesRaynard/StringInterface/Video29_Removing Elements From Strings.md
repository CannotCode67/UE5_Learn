
The `erase()` member function removes characters from the string. It takes two arguments, the first one is the index of the first element to be erased, and the second argument gives the number of elements to erase.
```
string str{"abcde"};
str.erase(3, 1);  // str now is "abce"
```
`erase()` with iterator
```
string str{"Hello"};
auto firstPos = begin(str);
str.erase(firstPos); // str now is "ello"
```
`erase()` with iterator range, it is a half open range, so the erase ends before the second iterator position.
```
string str{"Hello"};
auto firstPos = begin(str) + 1; // iterator pointing to 'e'
auto secondPos = end(str) -1; // iterator pointing to 'o'
str.erase(firstPos, secondPos); // "Ho"
```

`replace()` will replace some of the characters from a string with other characters.
```
string str{"Say Hello"};
auto pos = str.find("H");
str.replace(pos, 5, "Goodbye"); // replace 5 characters starting from 'H' with "Goodbye"
```
`replace()` with iterator range, it is a half open range, so the replace ends before the second iterator position.
```
string str{"Say Goodbye"};
str.replace(begin(str), begin(str) + 3, "Wave"); // "Wave Goodbye"
```
