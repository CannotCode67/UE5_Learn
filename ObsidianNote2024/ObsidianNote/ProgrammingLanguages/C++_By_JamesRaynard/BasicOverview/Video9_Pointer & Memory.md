A pointer is a variable whose value is a memory address (either on the stack or on the heap).

A pointer has type just like regular variable. For example, `int*` is a pointer type for `int`, meaning this pointer can only point to a memory address where an `int` content resides. And we should initialize the pointer as soon as it is declared, because interaction with a dangling pointer can crash the program or make the program behave strangely.

```
int i{1};
int* intP = &i;
cout << intP << endl; //output the memory address of i;
cout << *intP << endl; //output the content/value of i;
```
Example above, we initialize the pointer `intP` as soon as declaration. Through outputs, we can see the value of `intP` is the memory address, and we can get to the content through the pointer by putting a `*` sign (pronounced asterisk) in front of it. This process is called dereference.

In the above example, the pointer is pointing to a local variable, so pointed content is using memory from the stack, meaning the content will be gone once go out of scope. We can declare a pointer pointing memory from the heap by using the keyword `new`.
```
int* intP1 = new int; //intP1 points to memory allocated from the heap
int* intP2 = new int{36}; //intP2 is like intP1, but with initial value of 36
int* intP3 = new int(36); //the older versions of C++ for the same thing above
```
In the example above, all pointed memory address will remain allocated even the execution goes out of scope. Memory from the heap will remain allocated to the program until either we explicitly release it or the program terminates. The problem is, pointer itself is a local variable that will be automatically deleted, and once that happens, we no longer have access to the content which is pointed by our lost pointer. This is called memory leak. To avoid such a thing from happening, we need to release memory explicitly.

To release memory, we use the keyword `delete`. This keyword works with a pointer, and it deletes the pointed content. After the `delete` line and before the end of scope, accessing that dangling pointer will result in undefined behavior. That's why we see some programmers do defensive programming like `someP = nullptr;`.
```
int* intP = new int{36};
delete intP; //this line would delete the pointed content
intP = nullptr; //give a nullptr to intP, so it isn't dangling anymore
```

We can also allocate a block of memory and access it as if it were an array.
```
int* intArrayP = new int[20];
for (int i = 0; i < 20; ++i) {
	intArrayP[i] = i;
}
delete[] intArrayP;
```
In the example above, `new int[20]` will give back memory addresses for twenty integer values on the heap. To delete the whole pointed array, we need to add a `[]` right after the keyword `delete`.