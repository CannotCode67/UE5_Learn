
There are some advanced algorithms dealing with numeric processing, but they are in the `<cmath>` header. The following ones we look at are still under the `<numeric>` header.

`partial_sum()` writes the running total of the elements into another container. So given a source container `{a, b, c, ...}`, the target container will have `{a, a+b, a+b+c, ...}`.
```
vector<int> vec {1, 2, 3, 4, 5};
vector<int> vec1;
partial_sum(cebgin(vec), cend(vec), back_inserter(vec1));
```
Now, we have `vec1` of `{1, 3, 6, 10, 15}`.

`adjacent_difference()` writes the difference between successive elements into another container. So given a source container `{a, b, c, ...}`, the target container will have `{a, b-a, c-b, ...}`.
```
vector<int> vec {1, 3, 6, 10, 15};
vector<int> vec1;
adjacent_difference(cbegin(vec), cend(vec), back_inserter(vec1));
```
Now, we have `vec1` of `{1, 2, 3, 4, 5}`.

`inner_product()` multiplies the corresponding elements of two containers together and calculates their sums. So given two source containers `{a1, b1, c1, ...}` and `{a2, b2, c2, ...}`, the result will be `(a1*a2 + b1*b2 + c1*c2 + ...)`, equivalent to scalar product.
`auto result = inner_product(cbegin(vec), cend(vec), cbegin(vec1), 0);`
The `0` is the initial value to add on.

`inner_product()` is equivalent to `transform()` followed by `accumulate()`.
```
auto result = inner_product(cbegin(vec), cend(vec), cbegin(vec1), 0);
// above is equal to below
transform(cbegin(vec), cend(vec), cbegin(vec1), cend(vec1), back_inserter(vec2), std::multiplies<int>());
auto result = accumulate(cbegin(vec2), cend(vec2), 0, std::plus<int>());
```
So, `inner_product()` can take two predicates to override the underlying `transform()` and `accumulate()`.
```
inner_product(begin(vec), cend(vec), cbegin(vec1), 0,
				[](int n, int m) {...}, //this overrides accumulate()
				[](int n, int m) {...}); // this overrides transform()
```

