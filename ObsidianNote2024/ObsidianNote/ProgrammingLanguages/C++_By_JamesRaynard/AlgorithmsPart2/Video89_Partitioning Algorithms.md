
We can partition the elements in a container into two groups. Elements which meet some criteria at at the front of the container, and others are at the back of the container. The partition point marks the boundary between these two groups.

`std::partition()` takes an iterator range and a predicate, then it partitions the elements in that range based on the predicate. The elements within each group may not be in the order.
```
vector<int> vec {3, 1, 4, 1, 5, 9, 2, 8, 6};
partition(begin(vec), end(vec), [](int n){return n%2 == 1;});
```
Now, we have `vec` of `{3, 1, 9, 1, 5, 4, 2, 8, 6}`.

If we want them to keep their original relative order, we can use `stable_partition()`, but it is slower.
`stable_partition(begin(vec), end(vec), [](int n){return n&2 == 1;});`
Now, we have `vec` of `{3, 1, 1, 5, 9, 4, 2, 8, 6}`.

`is_partitioned()` takes an iterator range and a predicate function. It returns a bool depending on whether the range of elements is already partitioned by that predicate.
`is_partitioned(cbegin(vec), cend(vec), [](int n){return n%2==1;});`

`parition_point()` takes an iterator range and a predicate function. It returns the iterator to the first element where predicate returns false. Obviously, we need to check whether this range is already partitioned or not.
`auto ppoint = partition_point(cbegin(vec), cend(vec), [](int n){return n%2==1;});`