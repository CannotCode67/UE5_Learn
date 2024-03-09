
The `std::exception` is the base class. Under it, we have `bad_alloc` for memory allocating failure, `bad_cast` for dynamic cast failure, `logic_error` for errors we as the programmers can avoid, and `runtime_error` for errors we as the programmers cannot prevent. There are a couple of more specific exception classes under `logic_error` and `runtime_error`.

`std::exception` defines five public member functions, the constructor, copy constructor, assignment operator, virtual member function `what()` and virtual destructor. `what()` returns a C-Style string and cannot throw an exception.

`std::exception`, `bad_alloc`, `bad_cast` have a default constructor. Other exception classes have a constructor that takes in a string argument which can be a C-Style string or standard string. This argument will be stored as a private data member, and returned by `what()`. So if we get to write the code that throws such an exception, we should provide the string with information about the error condition.
```
int at(const vector<int>& vec, int pos) {
	if (vec.size() < pos + 1) {
		string str{"No element at index "s + to_string(pos)};
		throw std::out_of_range(str);
	}

	return vec[pos];
}
```

Under `logic_error`, we have:
`out_of_range`: attempting to access an element outside a defined range.
`invalid_argument`: the argument to a function is not acceptable.
`domain_error`: the argument to a function is outside the domain of applicable values.
`length_error`: the length limit of an object is exceeded.

Under `runtime_error`, we have:
`overflow_error`: the result of a computation is too large for the result variable.
`underflow_error`: the result of a floating-point computation is too small for the result variable.
`range_error`: an internal computation gives a value which cannot be represented by the result variable.

C++11 introduced some other sub classes of `std::exception`.

Under `bad_alloc`, we now have `bad_array_new_length` for error where we use `new` to allocate an array but the number of elements we pass is invalid. Maybe it is too big, and there is not enough memory.

`bad_weak_ptr`, `bad_function_call`, `future_error` under `logic_error`, and `system_error` under `runtime_error`.

The standard library defines all these exception classes, but itself rarely uses any of them. The reason is efficiency. The designers want the standard library to be as fast as possible. In some applications, if a library is not fast enough, then it is unusable.