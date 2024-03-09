
C++11 introduced smart pointers to replace the traditional pointers. They are RAII classes which encapsulate allocated memory, so they work with objects that live on the heap. We can still use traditional pointer but we should use smart pointer whenever it is possible. 

The study of traditional pointer is important as there are lots of codes using it, whether they are old codes written before C++11 or codes written specifically to use traditional pointers for whatever reason. As a C++ programmer, the bottom line is we should have no trouble understanding it when encounter codes with traditional pointer.

First attempt at a smart pointer was done in C++98 with `std::auto_ptr`. It fails because when we copy it, it transfers the allocated memory into the copy and its pointer is set to null automatically. People assumes if they are allowed to copy it, then there should be no modification done to the original. `std::auto_ptr` was removed from the language in C++14.

`std::unique_ptr` is basically an improved version of `auto_ptr`. It owns the memory, and cannot be copied or assigned to. But if we want to transfer its memory ownership, we can do that with `std::move()` as long as the target is also a `unique_ptr`.

`std::shared_ptr` can share its memory with other `shared_ptr` objects. The memory will be released only when there is no `share_ptr` pointing to it. A reference counting mechanism is implemented underneath to know how many `shared_ptr` is pointing to the same memory. It is very much like a garbage collector mechanism if you think about it. All these come with overhead price, so if we can use the `unique_ptr`. 