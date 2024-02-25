Because C++ is a both procedural and object-oriented language. That's why we have to have a `int main()` function to indicate the starting point of our program. And functions can be a member of a class, or just a free function. Luckily, the syntax for both are pretty similar.

// pass parameter by value or reference
The default behaviour for function is to pass parameter by value, meaning copying the parameter instead of using the parameter directly. We can explicitly denote that we want to use the parameter directly with a `&` sign.
```
int GetDouble(int x)
{
	x = x * 2;
	return x;
}

int DoubleInt(int& x)
{
	x = x * 2;
	return x;
}
```
Code above, the `&` added after the parameter type indicates this parameter will be used directly. However, unlike C#, when invocation, we don't write that `&` sign, so that is one of the differences. The pass by reference `&` sign can be used with the `const` keyword together like this.
```
int DoubleInt(int const& x)
{
	//...
}
```
Code above, in this case, we cannot modify x even though we are directly using x, not its copy. Well, you may think why would I bring a parameter in directly but not changing it? We save that for later. Mark a parameter `const&` can prevent complier to build, when we invoke methods on that parameter. 
```
string summary(string id, Person const& p)
{
	return format("{}: {} {}", id, p.GetName(), p.GetID());
}
```
Code above, we bring in Person p and mark it as `const&`, then we invoke its `GetName()` and `GetID()` methods. Those methods need to be marked as `const` for this code to complier, because the complier is basically saying the function summary promises not to change Person p, but those methods might change it. Judging from their names, we know they should be getters, and getters don't change the object. But complier doesn't read names, and we are not 100% sure either. So the way to go about this is to mark those methods with `const` keyword. Where do we do that? We need to mark them `const` both in their declaration and implementation.
Header file:
```
//...
class Person
{
	private:
		//...
	public:
		//...
		std::string GetName() const;
		int GetID() const {return int id;}
}
```
Implementation file:
```
#include "Person.h"
//...
string Person::GetName() const
{
	return firstName + " " + lastName;
}
```

// C# way to pass parameter by
This behaviour is very much like C#. C# is always pass by value as well, right? Well, when dealing with reference variable, meaning the variabel holding a memory address to point to the actual object in the heap, C# is pass by value as well, just the value happens to be the memory address, not the actual object in the heap. So the reference variable is copied into the function, and we can use that duplicated reference variable to modify the same actual object in the heap. If the function logic is to use the variable to modify the actual object in the heap, then the result would look as if we are passing by reference. However, if the function logic is to change the memory address, then, we would see the difference. In the case where we assign a new memory address through the new keyword, we are affective creating a new object in the heap, and assign corresponding memory address to the reference variable. If we pass the reference variable by defult, we get a copy of that reference variable and change the address value held by that copy, and when we end the function, the copy goes out of scope, and we lose the only way to find that newly created object in the heap, thus that object would get garbage collected. Which is not very useful, right? Normally, we want to assign the new address to the reference variable directly, so that would make a change. The way to modify the parameter by reference is to use `ref` keyword or `out` keyword, in both function declaration and invocation. `out` keyword requires the parameter to be uninitialized before the function.
Now, with all that recap, you still think C# handles passing parameter just the same as C++? The answer is no. When passing by value in the context of a class object, C# does a shallow copy, meaning copying the memory address of the reference variable. While C++ does copy the whole object, and thus the memory address is different as well.

// free function syntax
We know how to write a member function when we learn how to write a class. We separate the declaration into the header and the implementation into the cpp file. So the declaration of the function goes into the header file, and implementation goes into the cpp file. But what about free function? Especially coming from a strictly object oriented language like C#, that part is kind of confusing. Well, fear not, it is very similar.
Normally, we don't put the free functions into the main cpp file. We separate them out of the main cpp file like we do for classes. At the same time, those free functions need to be declared in a header file, and implemented in a cpp file, just like classes. Here is an example.
Header file called Utility.h, or HelperFunctions.h or whatever.h
```
#pragma once

bool GetDoubleInt(int x);
bool DoubleTheInt(int& x);
```
Cpp file called Utility.cpp or HelperFunctions.cpp or whatever.cpp
```C++
#include Utiltiy.h

int GetDouble(int x)
{
	x = x * 2;
	return x;
}

int DoubleInt(int& x)
{
	x = x * 2;
	return x;
}
```
Just like that, we have our free functions ready for `int main()`. It is like member functions, but without the whole class thing wrapping around.

// inline functions
When we hear inline functions, we usually think of anonymous functions in C#. But here is a little story about the real inline functions in C++. If we implement the functions along with its declaration inside the header file, complier would know this is a function to be inlined. However, in the binary file, complier might decide the implementation is where the invocation is or put the implementation somewhere else like what it does for regular functions. That's a decision made for you, not by you. Once upon a time, if we implement the function where we declare it, the complier would 100% put the implementation part to where the invocation is even though that is not an optimal thing to do. That's where the inline function get its name from. Nowadays, things are changed, but the names are stuck with us. Many people still implement small functions in the declaration as this is the only chance to ask the complier to inline them. Inline a small function usually has a performance benefit over looking up the function somewhere else, but again the outcome is decided by the complier.

// function signature
Function signature is used when we overloading the functions, so that the complier can know which one we are invoking even though the name is the same for them. C# and C++ is the same on this regard. We use parameter and return type together as function signature. 

// lambda function
We have lambda expression in C#, and it is just a very simplfied way to write an inline function. 
```C#
IEnumerable<string> filteredList = cities.Where(s => s.StartsWith("L"));
```

Well, we have the same thing in C++, but we call it lambda functions. And in fact, the syntax is more close to what we know as anonymous function in C#.
Anonymous functions in C#
```C#
IEnumerable<string> filteredList = cities.Where(delegate(string s)
											   {return s.StartsWith("L");});
```
Lambda functions in C++
```C++
int evens = std::count_if(begin(nums), end(nums), [](int i) {return i % 2 == 0;});
```
The `[]` sign is indicating this is a lambda function, and we define the parameter in `()`, and the implementation in `{}`. This `std::count_if()` function takes in a function as parameter to use as the check, if true, we count, if false, we do not.


// virtual function
When we deal with virtual function, this function is certainly a member function, as it can only gets virtual when there is inheritance involved. Virtual function under the hood has a small cost. Once we marked a function virtual, the complier would create a lookup table containing a set of function pointers, pointing to the implementation of the corresponding function for all possible versions in compile time. And in runtime, when we call the function through the object, each object has a pointer to this look up table and actually needs to look up the table to find the corresponding version to invoke.
Now, in C#, if we want to make a function virtual, we not only need to mark it as virtual in base class, but also need to override it in the derived class to really implement a derived class version. But in C++, things are a little different. We can mark a function virtual in the base class, and that alone is enough to make it work. No need for override keyword. Or we can mark a function virtual in the derived class, and that alone would also make it work as virtual function. However, C# way of marking base class method virtual and overriding it in derived class is a better way to do it. In C++, we can and we should handle virtual functions like how we do in C# because this would allow the compiler to catch error for you. The compiler doesn't allow overriding a method that is not marked as virtual in base class.
Now, let's see an example code, we only need to see the header, as that's the place where we put the `virtual` keyword, no need to do that in implementation file.
Person.h
```C++
//...
class Person
{
	private:
		//...
	public:
		//...
		virtual ~Person();
		virtual std::string GetName() const;
}
```

// virtual destructor
We discuss virtual function above, and we learn a base class reference or pointer can hold an object of derived class. When we invoke methods through a base class reference or pointer, we invoke different versions of the methods based on they are marked as virtual or not. The destructors get the same treatment, if we mark it virtual in the base class and override it in derived class, then when the object gets destroyed, its derived version of destructor gets invoked, otherwise, the base class version gets invoked. Why do we care? The derived class might have member pointer to address some heap living objects outside the class, and the clean up is the derived class destructor's responsibiltiy to do. In that case, we need to make sure it is the derived class's destructor gets invoked.


