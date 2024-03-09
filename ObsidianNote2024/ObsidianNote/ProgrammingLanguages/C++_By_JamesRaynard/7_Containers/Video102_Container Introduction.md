
Sequential containers store data in an order which is determined by the program, not by the value of the data. `std::vector` and `std::string` are examples of sequential containers.

Associative containers store data in an order which is determined by the value of the data. Under this category, we have sets and maps. A set is an unstructured collection of elements, where an element is a single data item consisting of a key. A map is similar to dictionary or hash map, where an element consists of a key and a value.

Because in an associative container, the position of element is determined by its key, so they don't support `push_back()` and `push_front()` operations.

The standard library also defines some container adaptors, like queues and stacks where data is store in an order which depends on when the data was added.

