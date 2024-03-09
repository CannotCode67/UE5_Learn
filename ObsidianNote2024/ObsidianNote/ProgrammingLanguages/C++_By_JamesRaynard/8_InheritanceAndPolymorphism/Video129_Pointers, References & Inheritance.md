
Normally, we have a pointer of `int`, then it must point to an `int` variable, so the pointer needs to have a type corresponding to the type of the variable or object.

However, a pointer to the base class can point to any object of any derived class. But not the other way around.
```
class Shape{};
class Circle : public Shape {};
Circle circle1;
Shape* pShape = &circle1; // this is allowed
Shape shape1;
Circle* pCircle = &shape1; // this is not allowed
```
Remember we learnt a derive class object is stored in memory as a base class object followed with its own specific part. A base class pointer can point to a derived class object because there is actually a base class object living there. But a derived class pointer cannot point to a base class object because there is no guarantee of the derived class specific part.

Another characteristics of pointing to a derived class object with a base class pointer is we can only access the base class's public members through the pointer.
```
class Shape{
public:
	void draw() {cout << "Generic shape";}
};
class Circle : public Shape {
int area = 10;
public:
	void draw() {cout << "circle shape";}
	void area() {return area;}
};
Circle circle1;
Shape* pShape = &circle1;
pShape->draw(); // display "Generic shape"
pShape->area(); // will not compile
```
The reason why is the compiler sees a `Shape` pointer, and recognizes the pointed object as `Shape`. We can write a bunch of global function to take in different specific shape objects and call their specific `draw()` functions. Or we can run a cast given we know what the object really is.
`static_cast<Circle*>(pShape)->draw() // this call the Circle's draw() function`

So, we give out the answer here first, the real solution is virtual function. But before we go learn virtual function, we need to understand static type and dynamic type.





