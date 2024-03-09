C++ is strongly-typed language just like C#. When we declare a variable, we need to give it a type, and this variable in most cases should hold a value of this type.

// number types
The fundamental types of C++ are a little bit complicated as some types are used back in the old days when memory is a limited resource. For example, when we want to hold a integer, we have four types to choose: short, long, longlong, int. Why? short is for small integer, long is for big integer, longlong is for huge integer. Those three also have unsigned version, meaning they don't hold negative numbers. Well, just go for int, when we want to hold a integer, like what we do in C#. 
For floating numbers with fractional parts, C++ has float, double, and long double. float has 7 decimal digits of precision, and double has 15. Just use float like what we do in C#.
If we use a int variabel to hold a value of float, the value of that variable is just the integer part of the float value, the fractional part is simply thrown away. This process is called truncation.

// character types
For words or letters, C++ has built-in type char to handle that kind of information, but it can only hold a single letter, which is not really practical these days. So most of time, we use the string class coming out of the standard library, it works just like string class in C#. By the way, char can be used to hold integer, but just a very small one, and it is not what char is for.

// boolean types
The bool type is to store boolean value, true or false. This one is pretty universal across all kinds of language.
In C++, the implicit conversion for number to boolean is 0 gets mapped to false, and everything else get mapped to true.

// key take away for using variable to store a value
When we use a variable to hold a value in an inappropriate way, meaning we use a char variable to store a number, or a int variable to store a float, etc., we can get value overflow, underflow, wraparound, and all kinds of unexpected behaviors. Yes, we can test it to see what really happens under the hood, but those tested behaviors can easily change when the application runs on a different operating system or on different hardware. Only when we use the tool in the way they are designed to be used, we gets the protection from the complier and have a consistent result.

// key take away for declaration and initialization
The way we do declaration and initialization as close as possible is considered a good habit. As we learn, uninitialized variable can be dangerous, especially pointers. The convention is called resource acquisition is initialization, RAII.

// var type, auto type
We know var type is the type dependent on the value in C#. We have the exact same thing in C++, is called auto type.

// casting
Now, sometimes, we like to do a casting to deliberately convert a type into another type. That's okay. But the C way of doing it: `float numF = (float)numInt;` is dangerous in C++ because unexpected behaviors happen under the hood. It should be okay in C# as that's the way we see people do casting in C#. In C++, the standard library provides mainly two functions to safely do casting: `static_cast<targetType>(variable)`, and `dynamic_cast<targetType>(variable)`. The `static_cast` does casting in compile time. But if you want to do the converting based on some user input, then this isn't the right tool. The `dynamic_cast` does casting in runtime, and it only works when casting a pointer to a class with a virtual table. If it succeeds, it returns a non-null pointer, if not, it returns a null pointer.

// const keyword
We can mark a variable as constant, so that once we initialize the variable, we can't change its holding value. Of course, we also need to do the initialization at the same time we do the declaration. It is very similar to what we have in C#, however, the place to put the keyword is different. In C#, we have `const int num = 1;`. And in C++, we have `int const num = 1;`. And this is not the only place for const keyword in C++, we use it a lot more frequently in C++ than we do in C#.
