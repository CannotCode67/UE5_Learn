
We use binary mode to work with binary files.
`ofile.open("image.bmp", fstream::binary);`

The `>>` and `<<` operators are not suitable here, because they perform conversions between numeric data and text. Also text data is formatted on output, and whitespace is ignored on input. Any of those will change the underlying data, so they are not good for binary files.

The operations we used are `write()` and `read()`. And we learnt they takes in a pointer to char for the first argument, and amount of data in bytes for the second.

The example data structure we use is a point.
```
struct point {
	char c;
	int32_t x;
	int32_t y;
}

point p{'a', 1, 2}; // used to write to a file
point p2; // used to read from a file
```
The reason we use fixed size is because there are different implementation for `int`, and we need to be definitive when working with binary files.

With our example data structure, the operations will look like these:
```
ofile.write(reinterpret_cast<char*>(&p), sizeof(point));
ifile.read(reinterpret_cast<char*>(&p2), sizeof(point));
```
When convert the pointer to our struct into a pointer to char, we use `reinterpret_cast` which will throw away the type information and just binary data. The alternative is the old way casting `(char*)(&p)`, but we should use the new standard one.

So all we need to know is we are using `write()` and `read()` to write and read data when dealing with a binary file. We need to match the argument types for those functions. We don't need to know how those data becomes binary data, that is the implementation details hidden in `fstream` object.

There is another concept we need to understand, which is word alignment. Modern hardware is optimized for accessing data which is word-aligned. On a 32-bit system, this means the address of each object is a multiple of 4. So what does that mean? In our example, `char` is one byte. `int32_t` is 4 bytes. Our struct is not word aligned. As `char` is not in a size of multiple of 4 bytes, so 0, 4, 8, 12, etc. `int32_t` is good though. The compiler is aware of this as well. Therefore, when compile, our struct will become below:
```
struct point {
	char c;
	char pad[3]; // this is the padding added by the compiler to be word aligned
	int32_t x;
	int32_t y;
}
```
Those padding bytes added by the compiler are considered not accessible by programmers despite some programmers will do hacky things with them, but it is not safe.

Most compilers provide a non-standard `#pragma` to allow programmers to have a mean to alter the padding.
```
#pragma pack(push, 1) // ask compiler to do padding as a multiple of 1
struct point {
	char c; // 1 byte, and not aligned
	int32_t x; // 4 bytes
	int32_t y; // 4 bytes
}
#pragma pack(pop) // reset the default compiler padding style
```
C++ has the `alignas` keyword to set padding on a variable basis.
```
struct point {
	char c;
	alignas(4) int32_t x; // only this use padding of 4, others use default padding
	int32_t y;
}
```
But the `alignas` keyword can only set the padding to the multiples of word size, not including 0. So on a 32-bit system, we can only pass 4, or 8 or 12, etc. The reason for that is some computers don't support non-word-aligned memory access, and C++ standard has to work on all computers ideally.

Now, we get the word alignment concept. It is time to see a full example.
```
struct point {
	char c;
	int32_t x;
	int32_t y;
}

int main()
{
	point p{'a', 1, 2}; // used to write to a file
	ofstream ofile("file.bin", fstream::binary);
	if (ofile.is_open()) {
		ofile.write(reinterpret_cast<char*>(&p), sizeof(point));
		ofile.close();
	}
	ifstream ifile("file.bin", fstream::binary);
	point p2; // used to read from a file
	if (ifile.is_open()) {
		ifile.read(reinterpret_cast<char*>(&p2), sizeof(point));
		ifile.close();
		cout << ifile.gcount() << endl; // 12 bytes, as we have the padding
		cout << p2.x << " " << p2.y << endl;
	}
}

```
