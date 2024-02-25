Array means an indexed block of contiguous memory, whether is on the stack or on the heap.

C++ inherits array from C, so array is part of the C++ core language. When we allocate an array on the stack, the number of elements has to be fixed and known at compile time.
```
int array[5]; // array of 5 ints allocated on the stack
int array[nElements]; // this is not allowed in standard C++, thus would not run
const int nElements = 5;
int array[nElements]; // this now works as nElements is constant now.
```

Dynamic array is array whose size we do not known at compile time. Or we want to be able to vary the number of elements of that array. Dynamic array must be allocated on the heap, so it looks like this.
```
int* arrayP = new int[nElements];
delete[] arrayP;
```
Since it is allocated on the heap, we have to explicitly release it when no longer needed, using keyword `delete[]`.

A C-Style string is an array of const char. C++ inherits it from C, and it is called string literals.
```
const char* str = "Hi"; // equivalent to const chat str[] = {"H", "i", "\0"}
```
The `\0` is called null character or terminating null, which has the numeric value of zero. So C-Style string iterate through the whole array to look for this null character to determine if we are reaching the end of string. Slow for sure, and there are many ways to lose this null character, so C-Style string is not safe.

Standard string is part of the C++ standard library, not the core language. However, if we need string, use this one. Unlike C-Style string being an array, std string is a class.
```
class std::string {
	char* data; // the array to store the content
	size_t n; // the size of the array
}
```
Object-oriented versus procedural. std string behave like a dynamic array, but are used like a local variable.
```
string hello{"Hello"}; // allocates memory on heap and populates it
```
The `std::string` class implements constructor and destructor in ways that will automatically use `new` keyword to get memory from the heap and `delete` keyword to clean up that memory. It also implements automatic reallocation of memory when its size varies. Since `std::string` still uses dynamic array to store content, `[]` (subscript notation) is supported to index its elements. The reason why the class has a member field to keep the size of the array, is we don't need to iterate through the whole thing to look for the null character to find out whether we reach the end of it. The `size()` member function is the getter for that field.

Standard vector is also part of the C++ standard library. It is similar to `std::string`, but can store data of any single type.
```
vector<int> intVec{4, 2, 3, 5}; // intVec is a vector of int
```
We put the type in `<>`, and it is known as the type parameter, short for `T`.
```
class std::vector<T> {
	T* data; // the array to store the content
	size_t n; // size of the array
}
```
Same as `std::string`, elements in `std::vector` are indexed. `size()` member function returns the number of elements. `push_back()` member function adds a new element at the back of the vector.