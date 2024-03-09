
Run-time type information (RTTI) relates to the dynamic type of the object. The `std::typeid()` function accepts an object then returns a `std::type_info` object which contains information about the dynamic type of an object. Both the `typeid()` function and the `type_info` class are defined in `<typeinfo>` header.

Let's see an example.
```
int main() {
	Circle circle;
	Shape* pShape = &circle;
	if (typeid(*pShape) == typeid(circle))
		cout << "same dynamic type" << endl;
	else
		cout << "different dynamic types" << endl;
}
```
We can store the `type_info` objects in variables.
`const type_info& typeInfo_Shape = typeid(*pShape);`

One member function `name()` returns a C-Style string representing the name of the class as used by the compiler. This compiler used name is unique for each dynamic type.
`cout << typeInfo_Shape.name() << endl;`

One member function `hash_code()` returns a unique number for each dynamic type.
`cout << typeInfo_Shape.hash_code() << endl;`

`dynamic_cast<>()` can convert a pointer/reference to base class into a pointer/reference to derived class. If it fails, it returns `nullptr`. For references, it throws `bad_cast` exception upon failure. 
```
int main() {
	Circle circle;
	Shape* pShape = &circle;
	Circle* pCircle = dynamic_cast<Circle*>(pShape);
	if (pCircle) {
		pCircle->circle_func();
	}
}
```
We know that to call a derived class version of a member function, all we need is to declare the member function as virtual in the base class. `dynamic_cast` is misused for this purpose when the programmer doesn't understand virtual function in C++. However, if the derived class has a member function which the base class doesn't have, we would need to `dynamic_cast` the base class pointer into a derived class pointer to invoke that member function. If we try to invoke that member function through a base class pointer, the compiler would try to find that member function in base class definition and fail, resulting a compilation failure. 