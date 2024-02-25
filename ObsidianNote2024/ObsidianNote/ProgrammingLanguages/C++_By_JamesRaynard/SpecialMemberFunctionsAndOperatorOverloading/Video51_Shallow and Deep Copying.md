
We give the answer first, then use an example to support it further. Shallow copying and deep copying are relevant when dealing with classes or objects that manage resources. Take the simplest example, a pointer pointing to a heap memory. A shallow copy of the pointer will result in two pointers pointing to the same memory, and interactions with them would just mean stomping on the same memory, interfering with each other. A deep copy on the other hand, would be coping the actual object that lives on that memory, and put the copy object in a new heap memory with a new pointer pointing to it.

We use a custom string storage class for example.
```
class String {
	char* data;
	int size;
public:
	String(const std::string& s): size(s.size()) {
		data = new char[size];
		for (int i = 0; i < size; ++i) {
			data[i] = s[i];
		}
	}

	~String() {
		delete[] data;
	}

	int Length() {
		return size;
	}
}
```

Now, if we do this.
```
int main()
{
	String str1("1"s);
	String str2("2"s);
	String str3(str2); // copy constructor is invoked
}
```
This will have a heap memory invalid error. When main reaches its end, local objects are destroyed in the reverse order of their creation. So `str3` destructor is invoked first, and `str1` destructor is invoked last. When `str3` destructor is invoked, it deletes the allocated memory which both `str3` and `str2` are pointing to. So, when `str2` destructor is invoked, it tries to delete the allocate memory that is already deleted, causing the error.
The solution is to write our own copy constructor.
```
String(const String& arg): size(arg.size) {
	data = new char[size];
	for (int i = 0; i < size; ++i) {
		data[i] = arg.data[i];
	}
}
```
Here we allocate new heap memory, and copy the char values over from the source object.

Now, if we do that.
```
int main()
{
	String str1("1"s);
	String str2("2"s);
	String str3(str2); // copy constructor is invoked
	str2 = str3; // assignment operator is invoked
}
```
Again, we have a heap memory invalid error. Because of the last line, now `str2` is pointing to the heap memory of `str3`, while losing its own original heap memory. When `str3` destructor is invoked, its heap memory gets deleted. Then, `str2` destructor is invoked, and it is trying to delete the heap memory of `str3` which of course, is already deleted.
The solution is to write our own assignment operator, and it is more complex than copy constructor. As you can see from example above, `str2` actually causes memory leak by losing its own original memory address. The original heap memory needs to be taken care of as well.
```
String& operator=(const String& arg) {
	if (&arg != this) {
		delete[] data; // take care original heap memory
		data = new char[arg.size];
		size = arg.size;
		for (int i = 0; i < size; ++i) {
			data[i] = arg.data[i];
		}
	}
	return *this;
}
```
There is a check `&arg != this`, and it is a safety check in case we assign the same object to itself like this: `str2 = str2;`. You may argue there is no point of doing that, and you are right. But in a very large and complex code base with multiple developers, there is a good chance this could happen. When that happens, if we take care of the original heap memory, we are actually delete the `arg`'s heap memory as well. And when we access `arg`'s heap memory next line, we get an error.
