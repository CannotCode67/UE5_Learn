
Insert iterators add new elements to a container. A example of that would be the output stream iterator. With regular iterator, we dereference it and assign a value would result in replacing whatever value in that position. But with insert iterator, we dereference it and assign a value would result in inserting the value in that position and push the rest back.

There are three types of insert iterators. 
`std::back_insert_iterator` adds an element at the back by calling the `push_back()` member function of the container.
`std::front_insert_iterator` adds an element at the front by calling the `push_front()` member function of the container.
`std::insert_iterator` adds an element at any given position.

Be aware not all containers support `push_back()` and `push_front()`, so insert iterators do not work for all containers.

To obtain an insert iterator, we call the inserter function, and pass the container object as argument.
`back_inserter(vec)` returns a `back_insert_iterator` for `vec`.
`front_inserter(vec)` returns a `front_insert_iterator` for `vec`.
`inserter(vec, regular_ite)` returns an `insert_iterator` for the `regular_ite` position of `vec`.

For insert iterators, we can use it multiple times without worrying they become invalid.
```
vector<int> vec;
auto it = back_inserter(vec);
*it = 99;
*it = 88;
```
Here, we insert two values with the same insert iterator. With a regular iterator, any modification to the container may cause the iterator to become invalid.

```
vector<int> vec {1, 2, 3};
auto ite2 = next(begin(vec));
auto insert_ite = inserter(vec, ite2);
*insert_ite = 99;
// vec now is {1, 99, 2, 3};
```
Here, after we insert `99` at the second position, the `ite2` becomes invalid, but the `insert_ite` can still work.
