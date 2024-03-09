
The standard library provides an `std::swap()` function to exchanges the data in its arguments. This function is declared as `noexcept`. Underneath, it tries to find an overloaded version of `swap()` for its argument's type, and if it cannot find one, then it uses the generic version which involve copying of its arguments.

We learnt the `std::string` version of `swap()` actually swaps the internal data buffers, so no copying, which is much faster.

Some algorithms in the library calls `swap()` underneath like `sort()` as they need to swap elements internally.

When we write our custom class, we should overload `swap()` when our class object is expensive to copy and we are expecting the class object will be swapped frequently.

The overloaded versions of `swap()` that come with the library are all mark `noexcept`. So algorithms use `swap()` underneath assumes your overloaded of `swap()` is also `noexcept`. The easiest way to make sure our overload is also `noexcept` would be to use our data member's version of `swap()`. We write the code inline so that the compiler can optimize away the function call.
```
class String {
private:
	int size;
	char* data;
public:
	...
	friend void swap(String& l, String& r) noexcept;
	...
};

inline void swap(String&l, String& r) noexcept {
	swap(l.size, r.size); // use the int version of swap
	swap(l.data, r.data); // use the C-Style string version of swap
}
```
