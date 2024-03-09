
We know that `shared_ptr` uses a technique called reference counting to know how many objects are referring to the same memory. In this video, we try to implement the technique ourselves.

We use our custom `String` class modified, so that its C-style array is shared, and it has a `counter` data member to count the reference.
```
class String {
private:
	int size;
	char* data;
	int* counter;
public:
	String(int size): size(size) {
		counter = new int{0};
		data = new char[size];
		++*counter;
	}
	~String() noexcept {
		--*counter;
		if (*counter == 0) {
			delete counter;
			delete[] data;
		}
	}
	String(const String& arg) {
		size = arg.size;
		data = arg.data;
		counter = arg.counter;
		++*counter;
	}
	String& operator= (const String& arg) {
		if (data != arg.data) {
			--*counter;
			if (*counter == 0) {
				delete counter;
				delete[] data;
			}
			size = arg.size;
			data = arg.data;
			counter = arg.counter;
			++*counter;
		}
		return *this;
	}
}
```
That is sufficient, but we can still add move operations on top.
```
String(String&& arg) noexcept {
	data = arg.data;
	size = arg.size;
	counter = arg.counter;
	arg.data = nullptr;
	arg.counter = nullptr;
}

frind void swap(String& l, String& r) noexcept;

String& operatot= (String&& arg) noexcept {
	String temp(std::move(arg));
	swap(*this, temp);
	return *this;
}
```