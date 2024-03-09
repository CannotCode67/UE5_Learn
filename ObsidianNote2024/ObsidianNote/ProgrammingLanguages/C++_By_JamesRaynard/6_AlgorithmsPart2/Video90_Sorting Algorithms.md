
We know the `std::sort()` put elements in a range into order. By default, it use `<` operator, but we can provide our own predicate.

`sort()` underneath is implemented as a quicksort, so elements with the same value may not keep their original relative order. If we want to maintain that order, we can use `stable_sort()`.
`stable_sort(begin(vec), end(vec));`

To reverse the order, we can tell it to use `>` operator in our own predicate.
```
sort(begin(vec), end(vec), [](int n){return m > n;});
```

`is_sorted` is like `is_partitioned()`, it returns a bool depending on whether the range is sorted.
`is_sorted(cbegin(vec), cend(vec));`

`is_sorted_until()` returns the iterator to the first element which is not in order.
`is_sorted_until(cbegin(vec), cend(vec));`

`partial_sort()` sorts a number of elements only, and keeps the rest unordered.
```
vector<int> vec {3, 1, 4, 1, 5, 9, 2, 8, 6};
auto it = advance(begin(vec), 5);
partial_sort(begin(vec), it, end(vec));
```
It looks at the whole range, and gives us the top 5 in order, and the rest unordered. So, we should have `vec` of `{1, 1, 2, 3, 4, 5, 9, 8, 6}`.

`partial_sort_copy()` sorts a number of elements and writes the data into target range. The number is defined by the target range.
`partial_sort_copy(cbegin(vec), cend(vec), begin(vec1), end(vec1));`

Sometimes, we have an iterator range unsorted, and we want to get the nth element as if it is sorted. We can do this in one step with `nth_element()`.
`nth_element()` takes the iterator range, and the iterator to nth element we want. After the operation, the iterator would point to a value which ranks nth in the whole range. And elements will be partitioned as if this nth element is the partition boundary. So elements before the nth element must be smaller or equal to it, and elements after the nth element must be bigger or equal to it. There may not be order within each group though.
```
vector<int> vec {3, 1, 4, 1, 5, 9, 2, 8, 6};
auto it = advance(begin(vec), 5);
nth_element(begin(vec), it, end(vec));
*it; // 4
// now vec is {1, 1, 2, 3, 4, 5, 6, 8, 9}
```