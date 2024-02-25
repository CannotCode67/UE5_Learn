
`std::transform()` will call a given function on every element in the range, and write the resulting data into a target container.
```
vector<int> vec {3, 1, 4, 1, 5, 9};
vector<int> vec1;
transform(cbegin(vec), cend(vec), back_inserter(vec1), [](int n){return 2*n};);
```
Now, we have `vec1` of `{6, 2, 8, 2, 10, 18}`.

Normally, algorithms check whether the target iterator range is overlapped with the original iterator range, if so, they don't perform the operations. But `transform()` is allowed to overlap both ranges. So we can do a in-place transformation.
```
transform(begin(vec), end(vec), begin(vec), [](int n){return 2*n;});
```
Now, we have `vec` of `{6, 2, 8, 2, 10, 18}`.

Another overload of `transform()` allows us to do operation with two ranges, then write the data to the third one.
```
transform(cbegin(vec), cend(vec), cbegin(vec1), back_inserter(vec2),
			[](int n, int m){return n+m;});
```
The problem is `transform()` doesn't check if `vec1` has enough elements to do this. We are responsible for that.