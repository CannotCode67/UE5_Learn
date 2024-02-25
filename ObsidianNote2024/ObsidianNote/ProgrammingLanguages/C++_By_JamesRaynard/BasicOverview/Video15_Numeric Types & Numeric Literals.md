
In C++, the size of built in types isn't precisely defined. It depends on the implementation. What does that mean? For example, when we use an `int` variable to store an integer value so big, the underlying type would be automatically replaced by something that can hold that value, like `long` or `long long`. So we don't know how many bits an `int` variable will be until we give it a value.

The only fixed built-in type is `char` which has a size of 8 bits. 
`int` has at least 16 bits.
`long` has at least 32 bits, and at least as many as `int` if `int` has a size equaling or bigger to 32 bits.
`long long` was introduced in C++11, and has at least 64 bits, and at least as many as `long` if `long` has a size equaling or bigger to 64 bits.

The C++ standard library offers something called C standard int, whose head is `<cstdint>`. They are brought in from C.
`int8_t` has a fixed size of 8 bits.
`int16_t` has a fixed size of 16 bits.
`int32_t` has a fixed size of 32 bits.
`int64_t` has a fixed size of 64 bits.
Unsigned version means they are only for positive numbers.
`uint8_t` has a fixed size of 8 bits.
`uint16_t` has a fixed size of 16 bits.
`uint32_t` has a fixed size of 32 bits.
`uint64_t` has a fixed size of 64 bits.

We can use `sizeof()` command to get the size of a type in bytes. 8 bits equal to 1 byte.
```
cout << sizeof(char) << endl; // we get a output of 1
```

We can directly use different number bases in C++.
If we give it a number, `int number = 42;` then C++ would assume 42 is a decimal number.
If we put `0x` in front, `int number = 0x2a;` then C++ would know this is a hexadecimal number.
If we put `0` in front, `int number = 052;` then C++ would know this is a octal number (base 8).
If we put `0b` in front, `int number = 0b101010;` then C++ would know this is a binary number.
However, if we `cout << number << endl;`, the output would choose to show a decimal equivalent. 

Floating-point types are like integer types, not precisely defined for their sizes.
`float` usually has 6 digits precision.
`double` usually has 15 digits precision.
`long double` usually has 20 digits precision.

Floating-point literals are double by default, so `3.14159` has a type of double.
Integer literals are int by default, but if the value is too big, then it might be a `long` or `long long`.

We can put a suffix at the back to force the literal to be certain type.
`3.14159f` has a type of float instead of double.
`123456789ULL` has a type of unsigned long long.

We can put a digit separator `'` in numeric literals to improve readability since C++14.
`1'000'000` is the same as `1000000`. But don't put it at the start or end of the literals.