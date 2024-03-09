
Exception safety means the program behaves safely when an exception is thrown. And there are three levels, basic exception guarantee, strong exception guarantee, and no exception guarantee.

Basic exception guarantee means if an exception is thrown, no resources will be leaked. So, a file gets closed or memory gets released. All the operations and functions in standard library provide this basic guarantee.

Strong exception guarantee means if an exception is thrown, the program can reverts back to the state before the operation as if the operation didn't happen. All standard library container operations provide this strong exception guarantee except for `insert()`.

No exception guarantee or no throw guarantee means the code doesn't throw any exception.

For any programmer, we should at least aim to achieve the basic exception guarantee. Normally, we can manually release the resource in the catch block. Or we can use objects which automatically release the resource when destroyed such as `std::vector` because all local variables in the current scope get destroyed when an exception is thrown. Another showcase of RAII.