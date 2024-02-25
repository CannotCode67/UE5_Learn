We have reference in C#, but no pointer. And we have both in C++. In some ways, the C# reference behaves more like the pointer in C++ rather than reference in C++. But there are still differences.

// reference in C++
```C++
int& rA = a;
rA = 5;
Person& rBlinn = Blinn;
rBlinn.SetID(456);
// &rBlinn can access the memory address value like how we get those for pointer
```
Code above shows that we need to use `&` sign to indicate this is a reference. In C#, we know a variable is reference when it is assign to an object in the heap. And the convention is to start the reference variable with a letter `r`. And through this reference variable, we can access the member with a `.` sign. 

A implicit characteristics of reference is once it is assigned with the memory address value, we cannot change that, all we can change is the object pointed by that memory address. And we have to initialize a reference when declaring it.

// pointer in C++
```C++
int* pA = &a;
*pA = 4;
pA = &b;
(*pA)++;
int* pB = pA;
Person* pBlinn = &Blinn;
(*pBlinn).SetID(456); // old way
pBlinn->SetID(456);
```
Code above shows that we need to use a `*` sign to indicate this is a pointer, and by convention pointer variable starts with a letter `p`. If we want to change the object it is pointing to, we need to put a `*` sign in front of the pointer, which is called dereferencing. If not, we are changing the pointer's memory address value. To get the memory address out of a value, we put a `&` sign in front of the value. But you see if we are assigning a pointer to another pointer, we need not to put a `&` sign in front. The way we access the members is through this `->` sign, also known as goto sign. There is also an old way to do it, dereference the pointer and access the members as if we are holding the object itself.

Unlike reference, we can change a pointer's memory address value apparently, and it can be uninitialized, which is super dangerous. Why? Well, because uninitialized pointer holds a random memory address, and other people including future self don't know if that is valid address or not. If we don't use the pointer when we declares it, we should always initialize it to null. A pointer initialized to null can be tested because if it points to null, we have a boolean value of false. How can we make it point to null? There are three ways to do it.
```C++
int* pInt = 0; // old way
pInt = NULL; // old way
pInt = nullptr; // modern way
if (pInt) // this check doesn't work with uninitialized pointer.
{
	*pInt = 3; // only when the pInt is not null, we dereference it to do work.
}
```

// const keyword with reference and pointer
We begin with the simple one, the referene. We know once the reference is declared and initialized, it cannot refer to another memory address. So reference itself is somewhat constant already. If we put a const keyword there
```C++
int const& rInt = 7;
```
Then, we can neither change the memory address of the reference nor change the object it refers to. Very constant indeed.

For pointer, adding the `const` keyword after the `*` sign would disable the pointer from refering to another memory address, and must be initilized when declared. On the other hand, adding the `const` keyword before the `*` sign would disable the pointer from modifying the object it refers to. Or we can add `const` keyword to both places, crazy right? In that case, the pointer is neither allowed to change memory address nor allowed to modify the refered object.
```C++
int* const pInt1 = &7; // not allowed to change memory address
(*pInt1)++; // allowed
int const * pInt2 = nullptr; // not allowed it to modify the refered object
pInt2 = &6; // allowed
(*pInt2)++; // not allowed
int const * const pInt3 = &8; // not allowed for either
pInt3 = &6; // not allowed
(*pInt3)++; // not allowed
```

// inheritance with reference and pointer
We can have reference variable or pointer variable of a base class type, and use it to store objects of derived class type. But not the other way around. However, when we access member functions, what do we get? The base class version or the derived class version? If you have a reference or a pointer perfectly matches the object's type, then we don't have to deal with this kind of ambiguity. However, we know in reality, we often try to achieve a generic solution, and having a base class variable is one way to do it. Now, in the land of C++, if you invoke a method through a base class reference or pointer, you will get the base class version unless that method is marked virtual, then in that case, you will get a derived class version of it.