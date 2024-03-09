
To establish an inheritance relationship, when we define a derived class, we put a `:` after its name, followed by the public keyword and the name of the base class.
`class Aeroplane : public Vehicle {...};`

The derived class will have access to all non-private members of the base class. Optionally, the derived class can replace the base class member functions with its own versions.

A derived class object is stored in the memory as a base object followed by the derived class part. When a derived class object is created, the base class's constructor is called first, then the derived class's constructor. When it is destroyed, the derived class's destructor is called first before the base class's.
```
class Aeroplane : public Vehicle {
public:
	Aeroplane() : Vehicle() {} //initialize the vehicle part of the object
}
```
If the constructors take in arguments.
```
class Vehicle {
int max_speed;
public:
	Vehicle(int max_speed) : max_speed(max_speed){}
};

class Aeroplane : public Vehicle {
int max_height;
public:
	Aeroplane(int max_speed, int max_height) : Vehicle(max_speed), max_height(max_height) {}
};
```

We can use a derived class as the base class of another derived class.
```
class Aeroplane : public Vehicle {...};
class Fighterplane : public Aeroplane {...};
```

