
As programmers, we often use resources.
heap memory;
files;
database connections;
GUI windows;
etc.

These resources need to be managed;
-allocate, open or acquire resource before we use it.
-release or close the resource after use.
-need to give special though about copying resource.
-need to think about error handling.

The standard library has some classes which manage resources, like `std::string` allocates memory for internal array. Like `std::fstream` binds a file. Like objects used in multi-threading. They all follow the encapsulation principle, so we are using the interface of those classes without knowing the implementation details on how to deal with resource. Another principle they follow is resource acquisition is initialization, short for RAII. It means the class constructor acquires the resource, and the class destructor releases the resource. One more principle they follow is, only one object can own a given resource at a time, which makes it impossible for different objects to step on each other's resources.