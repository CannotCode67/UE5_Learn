
`std::list` is a double-linked list where each element (node) consists of a pointer to the previous element, the value, and a pointer to the next element.

`std::list` has its own `insert()` and `erase()` member functions. They work like the generic ones, operating at the given iterator position, unlike `insert_after()` and `erase_after()`. Usually, with `std::list`, we use the member functions over the generic ones as the generic ones are either not supported or not optimized for `std::list`.
```
list<int> list1 {4, 3, 1};
auto last = end(l); // point to one position after 1
advance(last, -1); // advance -1 means moving to front by 1, point to 1 now
auto two = list1.insert(last, 2); // {4, 3, 2, 1}
//two is the new valid iterator to the second last place
list1.remove(two); // {4, 3, 1}
```

`std::list` are useful when we expect to add or remove elements frequently as such operations are faster than a sequential container like vector. With `std::list`, we have `push_back()` and `push_front()`, and they are both efficient. For `std::vector`, only the `push_back()` is efficient.

However, `std::list` cannot be indexed, so if we need to access a certain element, we have to access all elements until we reach our target, which is slow. The lack of random access also makes `std::list` not support the generic `sort()` algorithm, instead we use the `sort()` member function. The generic `remove()` doesn't really destroy the elements, and we need to call `erase()`. However, the `std::list` version of `remove()` would automatically call `erase()` for us.

Following are some `std::list` member functions.
`reverse()` would reverse the order of the elements.
`remove()` would destroy certain elements from the list.
`remove_if()` would destroy elements where predicate returns true.
`unique()` deletes duplicate elements from the list.

`merge()` will remove elements from the argument list, and merge them into the list which call the operation. If both lists are sorted before, then after `merge()`, the list will be sorted in ascending order.
```
list<int> list1 {1, 12, 6, 24};
list<int> list2 {9, 3, 14};
list1.sort(); // {1, 6, 12, 24}
list2.sort(); // {3, 9, 14}
list1.merge(list2); // {1, 3, 6, 9, 12, 14, 24}, list2 is empty now
```

`splice()` moves elements from another list into the given position of a list.
```
list<int> list1 {1, 12, 6, 24};
list<int> list2 {9, 3, 14};
auto p = begin(list1); // point to 1
advance(p, 1); // point to 12
list1.splice(p, list2); // {1, 9, 3, 14, 12, 6, 24}, list2 is empty now
```