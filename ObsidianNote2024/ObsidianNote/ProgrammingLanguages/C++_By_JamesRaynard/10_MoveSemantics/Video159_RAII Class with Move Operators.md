
In last video, we learn how to invoke the move versions of assignment operator and constructor. Here, we learn how to actually write those, and it is relatively easy once we understand the concept.

Back to our custom RAII class `String`.
```
class String {
private:
	int size;
	char* data;
public:
	...
}
```

Move constructor
```
String(String&& arg) noexcept {
	data = arg.data; // we shallow copy the array pointer
	size = arg.size;
	arg.data = nullptr; // get rid of the old pointer
	arg.size = 0;
}
```
We have to get rid of the old pointer, otherwise we have two pointers pointing to the same memory space. And as this object and the argument object both get their destructors called, we would get an error saying we try to delete the same memory twice. Calling `delete` on a `nullptr` has no effect, so this is the official way to do.

Move assignment operator
```
String& operator= (String&& arg) noexcept {
	if (this != &arg) {
		delete[] data;
		data = arg.data;
		size = arg.size;
		arg.data = nullptr;
		arg.size = 0;
		return *this;
	}
}
```
It is very similar to move constructor as we are actually doing the same thing, taking over the data members of the argument object. The difference is with move assignment operator, we have an existing object already. So we have to delete the original memory allocation first before we take the argument object's memory allocation.

Another way to write move assignment operator using copy and swap idiom
```
String& operator= (String&& arg) noexcept {
	String temp(std::move(arg)); // use the move constructor
	swap(*this, temp);
	return *this;
}
```