
`std::shared_ptr` was introduced together with `std::unique_ptr` in C++11, defined under the `<memory>` header. We know that its allocated memory is shared across its copy. And it uses reference counting to keep track of how many copies are there, and only releases the memory when the last one gets destroyed.

`shared_ptr` has two allocated memories, one is for the data, and the other is called control block for its reference counter and some other things. The suggested way to create and initialize a `shared_ptr` is to call `make_shared()` which would allocate those two memories in a contiguous memory block, so accessing them will be faster. 
`auto p1{make_shared<int>{42}};`
We can still call its constructor directly and pass a pointer.
`shared_ptr<int>p1{new int(42)};`

Following is common operations supported by `shared_ptr`.
```
int main() {
	shared_ptr<int> p1{new int(42)};
	auto p2{make_shared<int>(42)};

	*p1; // dereference it
	p1 = p2; // copy assignment operator
	shared_ptr<int> p3(p2); // copy constructor
	shared_ptr<int> p4(std::move(p2)); // move constructor
	p1 = nullptr; // invalidate it
}
```

A `unique_ptr` can be converted to a `shared_ptr`, but not vice versa. So we should always start with `unique_ptr` because it is more efficient, and able to be converted to `shared_ptr` if necessary.