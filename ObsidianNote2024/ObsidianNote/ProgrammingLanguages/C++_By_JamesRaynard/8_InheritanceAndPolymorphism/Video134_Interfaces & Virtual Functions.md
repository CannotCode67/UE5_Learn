
Base classes are usually some generic classes that represent a concept, and cannot be instantiated. Their virtual functions also usually have no concrete implementation. We can left them empty, or there is a C++ syntax to indicate they are pure virtual functions, meaning they don't have implementation in this base class, and derived classes must implement them.
```
class Shape {
public:
	virtual void draw() = 0; // this is a pure virtual function
};
```
If a class has a pure virtual function, then automatically, it becomes an abstract class. An abstract class cannot be instantiated. Derived classes must implement all pure virtual functions, otherwise, derived classes themselves become abstract as well.

When a function takes a base class object by value, we can pass a derived class object into it. But underneath, only the base class copy constructor gets invoked, and inside the function body, we only have a base class object even though we pass a derived class object. This is known as object slicing. If we make our base class abstract, the function cannot take it by value, but by pointer or reference. Then dynamic binding will be used, and our derived class object can go inside the body function intact.


