
We can overload inherited member functions, meaning we write a new member function which has the same name but a different signature.
```
class Vehicle{
public:
	void accelerate() {cout << "Increasing speed!";}
};

class Aeroplane : public Vehicle {
public:
	void accelerate(int height) {cout << "Accelerating at a height of " << height << endl;}
};

int main () {
	Aeroplane plane;
	plane.accelerate(1000); // display "Accelerating at a height of 1000"
	plane.accelerate(); // cannot compile
}
```
If we overload an inherited member function in the child class, it will hide all the other inherited member functions with that name. So we cannot call those functions on a child class object.

A naive way is to write a member function which is wrapper of the inherited member function.
```
class Aeroplane : public Vehicle {
public:
	void accelerate() {Vehicle::accelerate();} // wrapper function
	void accelerate(int height) {cout << "Accelerating at a height of " << height << endl;}
};
```
But the parent class might have many `accelerate()` member functions, and we might have a lot of unnecessary work. Since C++11, we can use the `using` keyword to tell the compiler to do those work for us.
```
class Aeroplane : public Vehicle {
public:
	using Vehicle::accelerate;
	void accelerate(int height) {cout << "Accelerating at a height of " << height << endl;}
};
```
It tells the compiler to unhide all the `Vehicle` member functions with name `accelerate` regardless their signatures. Although here, we overload one `accelerate()`, and if there is an `accelerate()` with the same signature, it will not be available as it is re-implemented.