
How to write exception safe class?
Take our RAII class `String` as an example.
```
class String{
private:
	int size;
	char* data;
public:
	...
}
```

The constructor of `String` allocates memory to store the C-Style string underneath. We know the keyword `new` can throw an exception, so the constructor cannot be `noexcept`. But there is no handler inside the constructor, so once an exception is thrown, the partially-created object will be destroyed. As a result, the constructor actually provides strong exception guarantee.
```
String(const char* str, int size): size(size) {
	data = new char[size];
	...
}
```

The copy constructor also allocate memory, so it is basically like the constructor. It also provides strong exception guarantee.
```
String(const String& arg) {
	size = arg.size;
	data = new chat[size];
	for (int i = 0; i < size; ++i)
		data[i] = arg.data[i];
}
```

The destructor would release the data memory, and `delete` doesn't throw, so no exception guarantee.
```
~String() noexcept {
	...
	delete[] data;
}
```

The assignment operator performs a deep copy which involves memory release and reallocation.
```
String& operator= (const String& arg) {
	if (&arg != this) {
		delete[] data;
		data = new char[arg.size];
		size = arg.size;
		for (int i = 0; i < size;, ++i) {
			data[i] = arg.data[i];
		}
	}
	return *this;
}
```
We delete the original memory space, then reallocate a new memory. If the keyword `new` throws an exception, we cannot go back to the previous state as the original memory space is already released. In order to achieve exception-safe, we have to rewrite it like below.
```
String& operator= (const String& arg) {
	if (&arg != this) {
		char* newdata = new char[arg.size];
		delete[] data;
		data = newdata
		size = arg.size;
		for (int i = 0; i < size;, ++i) {
			data[i] = arg.data[i];
		}
	}
	return *this;
}
```
We allocate the memory first, then release the original memory. If the keyword `new` throws an exception, we still have the original memory space. The drawback is now we need twice the memory space for this code to go through as for a short period of time, we have two memory allocations at the same time. This now provides strong exception guarantee.

Another way to write the assignment operator is to use copy and swap idiom. This also provides strong exception guarantee.
```
String& operator= (const String& arg) {
	String temp(arg); // this may throw
	swap(*this, temp);
	return *this;
}
```
The code is much cleaner and shorter, but we need more memory as we have a temporary `String` object now. If anything goes wrong with the constructor of `temp`, we still have everything just the way they are.
