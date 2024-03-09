
`std::remove()` takes in an iterator range and a given value, then it moves elements with that value to the back of the container, lastly those moved elements are given values which are not supposed to be used. By the end of the operation, the container will still have the same size as before.
```
vector<int> vec {3, 1, 4, 1, 5, 9};
auto defunct = remove(begin(vec), end(vec), 1);
vec.size(); // size is 6 as before
```
`remove()` returns the iterator to the first removed element. Now, `vec` has value of `{3, 4, 5, 9, ?, ?}`. Some compilers would not change the value of the removed elements, so they would still have a value of `1`. Others would do some optimizations resulting their having value of `{5, 9}`. The bottom line is we are not supposed to use those values.

`std::erase()` works with `remove()`, and `erase()` does destroy those elements. `erase()` takes in an iterator range, and destroys elements in that range, then changes the size of the container.
```
vector<int> vec {3, 1, 4, 1, 5, 9};
auto defunct = remove(begin(vec), end(vec), 1);
vec.size(); // size is 6 as before
vec.erase(defunct, end(vec));
vec.size(); // size is 4 now
```

The default behavior of `remove()` uses the `==` operator of the element type. If we want to do it differently, we can use `remove_if()` with our own predicate.
```
vector<int> vec {3, 1, 4, 1, 5, 9};
auto defunct = remove_if(begin(vec), end(vec), [](int n){return (n%3 ==0);});
vec.erase(defunct, end(vec));
```
Now, we have `vec` of `{1, 4, 1, 5}`.

`remove_copy()` would write the resulting data to a target container.
```
vector<int> vec {3, 1, 4, 1, 5, 9};
vector<int> vec1;
remove_copy(cbegin(vec), cend(vec), back_inserter(vec1), 1);
```
Now, we have `vec` unchanged, and `vec1` has a value of `{3, 4, 5, 9}`.

`remove_copy_if()` is similar, but takes a predicate.
```
remove_copy_if(cbegin(vec), cend(vec), back_inserter(vec1), 
				[](int n){return (n%3 ==0);});
```
Now, we have `vec` unchanged, and `vec1` has a value of `{1, 4, 1, 5}`.


`std::unique()` removes duplicate adjacent elements. In order to work properly, we should sort the container first. `unique()` is like `remove()` as it doesn't really destroy the removed elements. Instead, `unique()` moves those elements to the back and gives them a value not supposed to be used.
```
vector<int> vec {3, 1, 4, 1, 5, 9};
sort(begin(vec), end(vec)); // {1, 1, 3, 4, 5, 9}
auto defunct = unique(begin(vec), end(vec)); // size is still 6
v.erase(defunct, end(vec)); // {1, 3, 4, 5, 9}
```

`unique()` can take a predicate as third argument, but since we are providing logics to remove elements, this behaves just like `remove_if()`.
`auto defunct = unique(begin(vec), end(vec), [](int m, int n){return (n==m+1);});`

`unique_copy()` writes resulting data to a target container.
```
sort(begin(vec), end(vec));
unique_copy(cbegin(vec), cend(vec), back_inserter(vec1));
unique_copy(cbegin(vec), cend(vec), back_inserter(vec2),
			[](int m, int n){return (n==m+1);});
```
