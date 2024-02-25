
`std::reverse()` takes the elements in an iterator range and reverses their order.
`reverse(begin(vec), end(vec));`
There is also a `_copy()` version.
```
vector<int> vec {3, 1, 4, 1, 5, 9};
vector<int> vec1;
reverse_copy(cbegin(vec), cend(vec), back_inserter(vec1));
```
Now, we have `vec1` of `{9, 5, 1, 4, 1, 3}`.


`std::rotate()` performs a rotation about a given element.
```
vector<int> vec {1, 2, 3, 4, 5};
auto pivot = advance(begin(vec), 2); // pivot points to 3
auto res = rotate(begin(vec), pivot, end(vec)); // {3, 4, 5, 1, 2}
*res; // 1, the original first element
```

`rotate_copy()` writes the data to a destination, but for some reason, it doesn't work with an insert iterator. So we have to make sure the target container has enough space.
`auto res = rotate_copy(begin(vec), pivot, end(vec), begin(vec1));`
