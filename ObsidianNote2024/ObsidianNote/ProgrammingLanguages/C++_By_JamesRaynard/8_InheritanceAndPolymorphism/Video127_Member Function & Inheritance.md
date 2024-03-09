
A child class inherits all the non-private members of its parent class, including member functions. So we child class object can call its parent class's member functions.
```
class Vehicle {
public:
	void start() {cout << "Engine started!";}
};
class Aeroplane : public Vehicle {
};

plane.start(); // display "Engine started!"
```

A child class can re-implement the parent class's member functions.
```
class Vehicle {
public:
	void start() {cout << "Engine started!";}
};
class Aeroplane : public Vehicle {
public:
	void start() {cout << "Take off!";}
};

plane.start(); // display "Take off!"
```

We can re-implement the parent class's member functions as if we are to extend them.
```
class Vehicle {
public:
	void start() {cout << "Engine started!";}
};
class Aeroplane : public Vehicle {
public:
	void start() {Vehicle::start(); cout << "Take off!";}
};

plane.start(); // display ""Engine started!Take off!"
```

The `protected` keyword is like the other two accessors: `public` and `private`, but it allows the members to be accessed by child classes. We can see the `protected` members as interfaces to child classes.