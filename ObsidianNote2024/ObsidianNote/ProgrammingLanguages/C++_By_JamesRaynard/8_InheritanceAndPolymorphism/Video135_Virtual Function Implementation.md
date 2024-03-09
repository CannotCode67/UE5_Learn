
For normal member functions, we know they are implemented as global functions whose first argument is a pointer to the object.

For virtual member functions, it is quite different. When the compiler encounters a class with any virtual member function, it creates and populates a data structure known as `vtable` or virtual member function table. 

The `vtable` stores the address of all member functions of the class which are declared virtual. Underneath, it is an array of pointers to member functions. Each virtual member function is identified by an index into the table.

When the compiler sees a invocation of a virtual member function, it replaces the name of the function by the corresponding index in `vtable`, and generates some extra code to be executed at runtime. This piece of code checks the dynamic type of the object, locate the `vtable` for that dynamic type, look up the element in the `vtable`, dereference the function pointer to call it.

So virtual function has a overhead. Calling a virtual member function takes about 20-25% more time than calling a non-virtual one. The `vtable` and the function pointers need memory allocation. The extra flexibility has a price, so we should use it carefully.