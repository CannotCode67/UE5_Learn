
`std::deque` implements a double-ended queue, which is defined under the `<deque>` header. Double-ended means adding elements in the front and in the back are both efficient. 
`deque<int> dq{4, 2, 3, 5, 1};`

Underneath, it is implemented as a two-dimensional array like a vector. But unlike vector where the actual array is a contiguous memory block, deque has multiple memory blocks. The reason why is when deque runs out of memory space, it allocates new memory without modifying the old ones. So, it doesn't release the old memory space or copy data into new memory space. The result is as the data amount increases, deque will allocate multiple memory blocks to store them. The benefit is deque saves the instructions to release memory block and copy data around. However, deque is not optimized as much as vector, making it slower at most operations. Another reason for deque to be slow is modern hardware is optimized for access a contiguous block of memory, and with deque we have to jump around different memory blocks to access the data.

Therefore, to choose `deque`, we must need its `push_front()` efficiency, otherwise, we should use `vector`. List is much slower than vector and deque, only suitable when we need to frequently add and remove elements.

