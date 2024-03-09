
A function's executable code is stored in memory, so it has a memory address. We can get a pointer whose value is the memory address of the start of this code. This pointer is called function pointer.

How to get a function pointer?
```
void functionA(int, int);
auto function_Ptr = &functionA; //put a & will get the memory address of the function
```
The reason we use `auto` is because the type is finicky. The example here we would have the type `void (*)(int, int)`. But if we don't use `auto`, we would have to write it even more weirdly: `void (*function_Ptr)(int, int) = &functionA;`

A function pointer is a callable object. When it is called, the pointed function will be called.
How to call a function pointer? It is another ugly syntax.
`(*function_Ptr)(1, 2); // the * is optional`

Why don't we just call the function directly? A function pointer is a first class object, meaning we can pass a function pointer as argument to another function, and we can return a function pointer from another function. This is the foundation of callbacks, or event driven code.
When we pass function pointer as argument, we need to give it a type, and those voodoo above clearly cannot be put into the function argument. We have to use `using` declaration or `typedef` declaration to do that.
```
using pFunction = void (*)(int, int); // the * here is not optional
typedef void (*)(int, int) pFunction; // equivalent to the above line

void functionB(int x, int y, pFunction someFunctionPtr) {}
pFunction GetFunctionAPtr() {return &functionA;}

int main()
{
	auto function_Ptr = GetFunctionAPtr();
	functionB(1, 2, function_Ptr);
}
```

Function pointer was inherited from C, and thus it is a raw pointer, not a smart pointer. Therefore, function pointer can be overwritten, null or uninitialized. That makes it dangerous to use, and the ugly syntax just makes it worse.

There are better ways to achieve the same behavior.

