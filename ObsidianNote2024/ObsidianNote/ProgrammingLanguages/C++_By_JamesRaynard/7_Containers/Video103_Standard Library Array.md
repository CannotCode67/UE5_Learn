
The standard library defines `std::array` under the `<array>` header. It is a templated type where we need to provided two template arguments. The first one is the element type, and the second one is the size of the array. Once we call the constructor with those arguments, this `std::array` object can only store the given amount of elements in the given type.
`std::array<int, 5> arr{1, 2, 3, 4, 5};`

`std::array` object only lives on the stack. If we want something lives on the heap, we can use `std::vector`.

`std::array` is designed to replace the C-Style array, so it is like a C-Style array with improvement. `std::array` supports `size()`, so we don't need to keep track of the size. By the way, we can calculate a C-Style array's size by `int size = sizeof(array)/sizeof(array[0]);`. The memory size of the array divided by the memory size of an element will give us how many elements the array can store.

When passing a `std::array` object, we get the object itself. But with C-Style array, all of sudden, the array becomes a pointer to the first element of the array.

However, it still keeps the characteristics of the C-Style array. For example, `std::array` can store a fixed number of elements, so unlike `std::vector` where memory reallocation happens when space is not enough.