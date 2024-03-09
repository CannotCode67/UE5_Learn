
Integers can be represented by binary numbers. And bitwise manipulation of integers refers to the modification of an integer through operations done specific to its binary form.

C has operators for bitwise manipulation of integers. These are inherited by C++. But C++11 also  provides `std::bitset` under the `<bitset>` header to handle the same thing.

`std::bitset` is a templated type and its template parameter is the number of bits.
```
bitset<8> b1{"11110000"}; // 8 means it can store 8 bits
```
We can initialize it with a C-Style string like above, or with an integer, or binary constant.
```
bitset<8> b2{0xae}; // integer can be in decimal or hexadecimal
bitset<8> b3{0b10101110}; // binary constant
```

`std::bitset` has overload the `<<` and `>>` to work with streams, so it can be directly used to input or output.

`std::bitset` stores its binary data, and we can call member functions to get the data.
`to_ulong()` returns the data as an unsigned long.
`to_ullong()` returns the data as an unsigned long long.
`to_string()` returns the data as a C-Style string.

The `size()` member function gives the number of bits in the `bitset`.

We can access individual bits with subscript notation, but the index starts counting from the right to the left.
```
bitset<8> b1{"11110000"};
b1[0]; // rightest 0
b1[7]; // leftest 1
```
We can also access a bit by using `test()` member function. This function provides a bounds checking and if the checking fails, it throws a `std::out_of_range` exception.
```
b1.test(7); // leftest 1
b1.test(8); // throws exception
```

`std::bitset` provides member functions to manipulate bits.
`set()` will set all bits to true, and 1 is true.
`set(0)` will set the rightest bit to true.
`set(0, false)` will set the rightest bit to false.
`reset()` will set all bits to false;
`reset(0)` will set the rightest bit to false.
`flip()` will invert all bits.
`flip(0)` will invert the rightest bit.

`std::bitset` also provides member functions to check its own bits.
`all()` returns true if all bits are true.
`any()` returns true if at least one bit is true.
`none()` returns true if no bits are true.
`count()` returns the number of bits which are true.


