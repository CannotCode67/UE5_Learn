
Some algorithms are used for numerical processing of container elements, and they are not under the `<algorithem>` header but the `<numeric>` header.

`std::iota()` takes an iterator range and a starting value, then it populates that range with values which successively increase by 1.
```
vector<int> vec(10); // a vector of ten-integer space
iota(begin(vec), end(vec), 1);
// now vec is {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
```


`std::accumulate()` returns the sum of all elements in the given iterator range. Its third argument is the initial value to add on, and not optional. If we just want the sum of the elements, then we put `0` for the third argument.
`auto sum = accumulate(cbegin(vec), cend(vec), 0);`
We can also give it the fourth argument to replace the default `+` operator.
```
auto sum = accumulate(cbegin(vec), cend(vec), 0, 
					[](int sum, int n) { return (n % 2 == 1)? sum + n : sum;});
```
Here, we only add the element if it is odd.

`std::reduce()` was introduced by C++17. It is an alternative of `std::accumulate()`. It works the same, but it is compatible with parallel processing.