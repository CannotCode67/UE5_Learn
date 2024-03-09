
Copy algorithms are often used to populate sequential container.

`std::copy()` is the most basic copy algorithm.
```
vector<int> vec1 {3, 1, 4, 1, 5, 9};
vector<int> vec2(vec.size());
copy(cbegin(vec1), cend(vec1), begin(vec2));
```
It doesn't check if the target container is big enough. So a better way to use it is with an insert iterator.
```
vector<int> vec1 {3, 1, 4, 1, 5, 9};
vector<int> vec2; // vec2 is an empty container
copy(cbegin(vec1), cend(vec1), back_inserter(vec2));
```

`std::copy_n()` will only copy the first `n` elements of the iterator range.
```
vector<int> vec1 {3, 1, 4, 1, 5, 9};
vector<int> vec2; // vec2 is an empty container
copy_n(cbegin(vec1), 2, back_inserter(vec2));
```
Now, we have `vec2` of `{3, 1}`.

`std::copy_if()` copies only the elements where predicate returns true.
```
vector<int> vec1 {3, 1, 4, 1, 5, 9};
vector<int> vec2; // vec2 is an empty container
copy_if(cbegin(vec1), cend(vec1), back_inserter(vec2),
		[](int n){return (n%2 == 1);});
```
Now, we have `vec2` of `{3, 1, 1, 5, 9}`.