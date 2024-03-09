
The static type in C++ is the type used to declare the variable.
```
int x{5}; // static type int
int* pInt = &x; // static type pointer to int

Circle circle; // static type Circle
Circle* pCircle = &circle; // static type pointer to Circle
Circle& refCircle = circle; // static type reference to Circle

Shape* pShape = &circle; // static type pointer to Shape
Shape& refShape = circle; // static type reference to Shape
```

Dynamic type in C++ is the type of the actual variable living in the memory. For most objects, this will be the same as the static type except when a parent class pointer or reference points to a derived class object.
```
int x{5}; // dynamic type int
int* pInt = &x; // dynamic type pointer to int

Circle circle; // dynamic type Circle
Circle* pCircle = &circle; // dynamic type pointer to Circle
Circle& refCircle = circle; // dynamic type reference to Circle

Shape* pShape = &circle; // dynamic type pointer to Circle
Shape& refShape = circle; // dynamic type reference to Circle
```

C++ almost always uses static typing, resulting less runtime overhead and more optimization done by the compiler. When a parent class pointer or reference points to a derived class object, the compiler does not decide which member function to call. Instead, the decision is made using object in memory at runtime.

To use dynamic type, we declare the member function as virtual in the base class. When we invoke the function with that name through a pointer/reference of a base class, the dynamic type of the pointed object will be used to decide which version of the function to be called.
```
class Shape{
public:
	virtual void draw() {cout << "Generic shape";} // declare with virtual
};
class Circle : public Shape {
int area = 10;
public:
	void draw() {cout << "circle shape";}
	void area() {return area;}
};
Circle circle1;
Shape* pShape = &circle1;
pShape->draw(); // display "circle shape"
pShape->area(); // will now compile
```