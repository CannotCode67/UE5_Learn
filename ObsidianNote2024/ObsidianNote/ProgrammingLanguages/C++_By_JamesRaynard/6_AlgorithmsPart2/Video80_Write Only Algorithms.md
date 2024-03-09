
Standard Library provides some write only algorithms mainly for the purpose of populating sequential containers.

`std::fill()` assigns a given value to the elements in an iterator range.
```
vector<int> vec(10);
fill(begin(vec), end(vec), 42);
```
Here, we end up with `vec` has 10 elements of `42`.

`std::fill_n()` is similar, but it takes a number which is how many elements to fill the value with. And it returns the iterator to the element after the last one assigned.
```
auto begin_rest = fill_n(begin(vec), 5, 42);
fill(begin_rest, end(vec), 99);
```
Here we end up with `vec` has `{42, 42, 42, 42, 42, 99, 99, 99, 99, 99}`.

One problem with `std:fill_n()` is it doesn't check the container's size. If we give it a container without enough space, we would crash the program. Two ways to handle that. First, we can check if the vector is big enough, if not we resize it manually.
```
if (vec.size() < 5) {
	vector<int> new_vec(5);
	vec.swap(new_vec); // this would swap the actual array
}
```
Or we can use an insert iterator instead of a regular iterator.
```
auto begin_rest = fill_n(back_inserter(vec), 5, 42);
```
Because we are calling `push_back()` underneath this way, the underlying array will be resized if there isn't enough space. So we will not crash.

`std::generate()` takes in an iterator range and callable object, and it populates the range with value given by the callable object. We can use function pointer, functor, and lambda.
```
vector<int> vec(10);
auto square = [](int n) { ++n; return n*n;};
generate(begin(vec), end(vec), square);
```

`std::generate_n()` is like `fill_n()` but in the version of `generate()`.
```
vector<int> vec(10);
generate_n(back_inserter(vec), 10, square);
```