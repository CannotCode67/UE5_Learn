
Normally, when we call a member function of an object, the compiler will decide which function is called using the static type of the object. This is known as static binding.

When we invoke the function with that name through a pointer/reference of a base class, the dynamic type of the pointed object will be used to decide which version of the function to be called. This is known as dynamic binding.

To make a member function virtual, we put the `virtual` keyword before its declaration in the base class. `virtual void draw(){}`
This virtual member function will be inherited in all sub classes. If we need to override this virtual function, we can just re-implement it.
```
class Shape {
public:
	virtual void draw() const {};
	...
};

class Circle : public Shape {
public:
	void draw() const {};
	// void draw() const override {};
...
};

Circle circle;
Shape& refShape = circle;
refShape.draw(); // invokes the draw() of Circle class
```
There is an `override` keyword introduced in C++11, and we can put it in the declaration in the derived class. It is not necessary for overriding to happen, but it would ask the compiler to check if there is any virtual function with the same name and the same signature to override. As we can see in the example, a typo can make the override of a virtual function becomes a declaration of a new function. With `override`, we are explicitly telling the compiler that we are overriding a virtual function, not declaring a new function.

C++11 also introduced the `final` keyword which we can use to decorate a class or a member function. A class with `final` means it cannot be derived from, so it cannot be used as a parent class. A member function with `final` means it cannot be overridden in child classes. Libraries often use `final` to prevent someone from extending their content.
```
class Shape {
public:
	virtual void draw() const;
	...
};

class Circle final : public Shape {
public:
	void draw() const override final;
...
};

class DeluxeCircle : public Circle {} // this will not compile
```