
`std::string` and `std::vector` have a `data()` member function which returns a pointer to the container's internal memory buffer, so the actual array. For `std::string`, the array is null-terminated like C-Style string. You might think `data()` as the getter for this member field. It is useful when dealing with C code.
```
void print(int* arr, size_t size); // represents a C API
std::vector<int> numbers {1, 2, 3, 4, 5};
print(numbers.data(), numbers.size());
```

`std::string` has a `swap()` member function.
```
string str1{"Hello"};
string str2{"Goodbye"};
str1.swap(str2); // str1 has a value of "Goodbye", and str2 "Hello"
swap(str1, str2); // a global version is provided as well
```

The global `swap()` has overloads for all built-in and standard library types. If there is a type it doesn't know how to deal with, it uses the default implementation below.
```
Obj temp = obj1;
obj1 = obj2;
obj2 = temp;
```
We know how inefficient for `std::string` to use the approach above. The `swap()` overload for `std::string` will just swap the size member field and the array pointer, no actual swapping for any of the characters. This is known as move semantics.