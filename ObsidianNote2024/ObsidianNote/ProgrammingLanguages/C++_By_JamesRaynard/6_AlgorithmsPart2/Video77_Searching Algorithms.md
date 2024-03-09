
`std::string` has a member function `find_first_of()` to search if the string contains any element of another string. If it finds a match, it returns the index of the first match.
```
string str {"Hello World"};
string vowels {"aeiou"};
str.find_first_of(vowels);
```

There is a generic algorithm function `std::find_first_of()` that works the same but compatible with all containers that support iterator range.
`auto vowel = find_first_of(cbegin(str), cend(str), cbegin(vowels), cend(vowels));`
It returns an iterator to the first match. It uses the `==` operator of the element type.

`std::adjacent_find()` looks for two neighboring elements that have the same value. If it finds a match, it returns an iterator to the first element of the two.
`auto pos = adjacent_find(cbegin(str), cend(str));`

`std::search_n()` looks for a sequence of `n` successive elements which have the same given value. If it finds a match, it returns the iterator to the first element of the sequence.
```
vector<int> vec {1, 2, 2, 3, 2, 3, 3};
// look for a sequence of two elements with value 3
auto pos = search_n(cbegin(vec), cend(vec), 2, 3);
```

`std::search()` takes two iterator ranges, and looks for an occurrence of the second iterator range inside the first range.
```
string str {"Hello world"};
string sub {"wo"};
// look for "wo" in str
auto pos = search(cbegin(str), cend(str), cbegin(sub), cend(sub));
```