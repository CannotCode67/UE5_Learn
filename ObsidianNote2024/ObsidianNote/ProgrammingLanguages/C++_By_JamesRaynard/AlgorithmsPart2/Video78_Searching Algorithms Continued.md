
`std::mismatch()` takes two iterator ranges, and looks for differences between the two ranges. If it finds a mismatch, then it returns a pair of iterators to the first element that has a different value in each range.
```
vector<int> vec1 {1, 2, 2, 3, 2, 3, 3};
vector<int> vec2 {1, 2, 2, 2, 2, 3, 3};

auto elems = mismatch(cbegin(vec1), cend(vec1), cbegin(vec2), cend(vec2));
*elems.first; // 3
*eleme.second; // 2
```

The following three algorithms take an iterator range and a predicate.
`std::all_of()` returns true if the predicate is true for all elements.
`std::any_of()` returns true if the predicate is true for at least one element.
`std::none_of()` returns true if the predicate is false for all elements.
```
vector<int> vec {3, 1, 4, 1, 5, 9};
auto is_odd = [](int n) { return n % 2 == 1;};
all_of(cbegin(vec), cend(vec), is_odd);
any_of(cbegin(vec), cend(vec), is_odd);
none_of(cbegin(vec), cend(vec), is_odd);
```

`std::binary_search()` is a much quicker alternative of `find()`, but it requires the elements inside the given iterator range are sorted.
```
vector<int> vec {3, 1, 4, 1, 5, 9};
sort(begin(vec), end(vec));
binary_search(cbegin(vec), cend(vec), 4);
```

`std::include()` takes two iterator ranges, and it also requires the elements in both given iterator ranges are sorted. It returns a bool depending on whether all the elements in the second range are present in the first range.
```
string str {"Hello World"};
string vowels {"aeiou"};
sort(begin(str), end(str));
sort(begin(vowels), end(vowels));
includes(cbegin(str), cend(str), cbegin(vowels), cend(vowels));
```