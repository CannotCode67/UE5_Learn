
`std::forward_list` has a better known name, singly linked list. Each element (node) in a list has its own memory allocation, and it consists of the value and a pointer. The pointer stores the memory address of the next element. The last element has an invalid pointer.
`forward_list<int> forwardList {4, 3, 1};`

`std::forward_list` only supports forward iterators, so `insert()` and `erase()` functions are not supported as they require container to support reverse iterators.

Instead, `std::forward_list` has `insert_after()` and `erase_after()` member functions. Both functions take an forward iterator, and `insert_after()` takes one more argument, the value to insert. But as the name suggests, if you give it an iterator to the second position, `insert_after()` will insert the value after that, so as the third element. Same thing with `erase_after()`, if you give it an iterator to the second position, it would destroy the third element. That's different from the regular `insert()` and `erase()` functions.
```
forward_list<int> forwardList {4, 3, 1};
auto second = advance(forwardList.begin(), 1); // point to 3
forwardList.insert_after(second, 2); // {4, 3, 2, 1}
forwardList.erase_after(second); // {4, 3, 1}
```

`splice_after()` moves elements from another list into the given position of a list.
```
forward_list<int> list1 {1, 12, 6, 24};
forward_list<int> list2 {9, 3, 14};
auto p = begin(list1); // point to 1
list1.splice_after(list2); // {1, 9, 3, 14, 12, 6, 24}
```