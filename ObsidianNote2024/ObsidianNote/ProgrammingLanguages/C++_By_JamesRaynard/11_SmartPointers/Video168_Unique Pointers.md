

`unique_ptr` is defined under the `<memory>` header. It is a template type and a move-only class.
```
unique_ptr<int> p;
```

To initialize a `unique_ptr`, we can call `new` explicitly.
```
unique_ptr<int> uniPtr_int{new int{42}};
unique_ptr<int> uniPtr_array{new int[6]};
```
Or we have `make_unique()` introduced in C++14.
```
auto uniPtr_int{make_unique<int>(42)};
auto uniPtr_array{make_unique<int[]>(6)};
```
`make_unique()` uses perfect forwarding.

`unique_ptr`'s interface makes sure many but not all operations with traditional pointers are supported. And its interface is different when it points to a single object or an array.
```
unique_ptr<int> uniPtr_int{new int{42}};
unique_ptr<int> uniPtr_array{new int[6]};

auto uniPtr_int1{make_unique<int>(24)};
auto uniPtr_array1{make_unique<int[]>(6)};

*uniPtr_int; // dereference is ok when point to single object
*uniPtr_array; // will not compile when point to an array
uniPtr_array[2]; // can be indexed when point to an array
unique_ptr<int> uniPtr_int2(uniPtr_int1); // copy is not allowed
unique_ptr<int> uniPtr_int2(std::move(uniPtr_int1)); // move with same type is ok
unique_ptr<int> uniPtr_int3(std::move(uniPtr_array1)); // not ok
uniPtr_int = nullptr;
```
Assigning `nullptr` to a `unique_ptr` will invalid this `unique_ptr` and release its memory.

We can access data member using go-to sign just like before.
```
struct Point {
	int x;
	int y;
};

int main() {
	auto uniPtr{make_unique<Point>(Point{3, 6})};
	uniPtr->x; // 3
	uniPtr->y; // 6
}
```

Because `unique_ptr` is move-only class, then it is passed into a function, it must be passed by move.
```
void func(unique_ptr<Point> upp) {
	upp->x;
	upp->y;
}

auto uniPtr{make_unique<Point>(Point{3, 6})};
func(std::move(uniPtr));
uniPtr->x; // will crash
```
`func()` is written in passed by value syntax, so it will use passed by move when the passed object is a `rvalue` and moveable. `unique_ptr` is a move-only class, but `uniPtr` is a `lvalue`. So we have to use `std::move().
After the operation, `uniPtr` loses its data, so attempt to access its member will crash the program.

What if we want `func()` to do something with the data for `uniPtr` without invalidating it? Well, we can return a `unique_ptr` from a function with move semantics.
```
void func(unique_ptr<Point> upp) {
	upp->x;
	upp->y;
	return upp;
}

auto uniPtr{make_unique<Point>(Point{3, 6})};
uniPtr = func(std::move(uniPtr)); // this should call move assignment operator
```
There are two move operations happening. The `upp` is moved into the function's return space, then its memory is transferred to the temporary object in return space. Then the temporary object in return space is moved again into the `uniPtr`. We don't need to cast `upp` into a `rvalue` as the compiler will always optimize to return by move whenever possible.