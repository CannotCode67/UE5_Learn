
`std::replace()` changes all elements of a given value to another value in a given iterator range.
```
vector<int> vec {3, 1, 4, 1, 5, 9};
replace(begin(vec), end(vec), 1, 2);
```
Now, we have `vec` of `{3, 2, 4, 2, 5, 9}`.

`replace_if()` changes elements where predicate returns true.
`replace_if(begin(vec), end(vec), [](int n){return (n%2 == 0);}, 6);`
Above, we replace any even number with a value of `6`.

Write algorithms usually have a `_copy()` version which writes data to a different iterator range, so the original data is preserved.
```
vector<int> vec {3, 1, 4, 1, 5, 9};
vector<int> vec1;
replace(begin(vec), end(vec), back_inserter(vec1), 1, 2);
```
Now, we have `vec` unchanged, and `vec1` of `{3, 2, 4, 2, 5, 9}`.

`replace_copy_if()` would writes the data to a target iterator range, and gives a new value to the elements where predicate returns true.
```
replace_copy_if(cbegin(vec), cend(vec), back_inserter(vec1), 
				[](int n){return(n%2==0);}, 6);
```
Now, `vec` is unchanged, and `vec1` will have `{3, 1, 6, 1, 5, 9}`.
