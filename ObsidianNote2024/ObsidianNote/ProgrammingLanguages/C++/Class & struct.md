Class in C++ is very different in the way they are structured. In C#, we declare a class and implement its fields, methods, properties in one place. However, in C++, in order to really have a class, we write a class header, and the class implementation in two separated files.

The class header file, usually has a extension of h or hpp. A example here would be like Person.h. And inside the class header, we write
```
class Person
{
	private:
		std::string firstName;
		std::string lastName;
		int idNumber;
	
	public:
		std::string GetName();
};
```
As you can see, we don't provide a accessor in front of the class. I guess it must be treated as public then. And inside, we write two sections: public and private, the order doesn't matter, and if we don't provide an accessor, the default for class is private. In those sections, we do declaration, no implementation. We can do implementation here actually, but the way to do it is to write implementation in a separated cpp file. Also, we need to provide a semicolon after the closing bracket, very easy to miss.

Let's see what we write in the class implementation file, by convention this cpp file has a name of the class, here would be Person.cpp.
```
#include "Person.h"

std::string Person::GetName()
{
	return firstName + " " + lastName;
}
```
The first line is called header, and you may think it is something like using statement in C#. Well they are still slightly different. Here, it works as a link to the the header file. We then implement the method, we have to use the full name for the method `Person::GetName()`. But inside the method, we can directly access the fields just as if we are inside the class. Very weird, right? That's C++. 

// key take away
![[buildProcess.png]]
The header file is a promise to the complier that there is a class containing such following members. And It is linker's job to find those corresponding implementation. If we have a header file without an implementation file, when we build, we have an error code starting with L, indicating it is the linker complaining. If we don't have a header included inside the file where we use the class, we have an error code starting with C, indicating it is the complier complaining.

// Constructors
Now, the above header file and implementation file does not contains a constructor, let's add one.
Header file:
```
class Person
{
	private:
		std::string firstName;
		std::string lastName;
		int idNumber;
	
	public:
		Person(std::string first, std::string last, int num);
		std::string GetName();
};
```
Implementation file:
```
#include "Person.h"

Person::Person(std::string first, std::string last, int num)
	: firstName(first), lastName(last), idNumber(num)
{}

std::string Person::GetName()
{
	return firstName + " " + lastName;
}
```
You may notice the weird syntax for constructor implementation. Actually, we can do it the way we do in C# like this.
```
Person::Person(std::string first, std::string last, int num)
{
	firstName = first;
	lastName = last;
	idNumber = num;
}
```
The difference is speed. The first way is the recommended way for C++, it is faster. The reason behind, is when we call the constructor of a class, under the hood each member's constructor is called as well. The first way is tell the member's constructor to directly construct itself with the passed in value. While the second method, we need to copy the parameter values and give them to the fields. The `=` sign in C++ is called a copy assignment operator. Under the hood, the first value is copied, and then given to firstName, then the first value is discarded. While the first way, there is no copying, just take the first value and give it to the firstName's constructor. Only the first variable gets discard in the end. I guess this is the reasoning behind. I am not sure.

If we don't write any constructor, a default one will be provided. But once we provide a custom constructor, the default one will no longer be provided. Just like C#.


// The way to use a class is also different
In C#, when we want to create a class, we write `person = new Person(first, last, 1234);`. We use the new keyword. But in C++, we do it like this `Person person(first, last, 1234);`. Notice that, there is no new keyword or equal sign, and the variable name is after the Class type. To access the member, we still use the `.` sign like this `std::string name = person.GetName();`. 

// Destructors
Destructor is like constructor, if we don't provide a custom one, a default one is provided for us. However, many times, a default destructor is really not enough, especially in C++ where manual memory management is involved. Anyway, let's see a destructor in its simplest form.
Header file:
```
class Person
{
	private:
		std::string firstName;
		std::string lastName;
		int idNumber;
	
	public:
		Person(std::string first, std::string last, int num);
		~Person();
		std::string GetName();
};
```
Implementation file:
```
#include "Person.h"

Person::Person(std::string first, std::string last, int num)
	: firstName(first), lastName(last), idNumber(num)
{}

Person::~Person()
{}

std::string Person::GetName()
{
	return firstName + " " + lastName;
}
```
A destructor has the name of the class, but with a tilda sign in front. Unlike constructor, destructor is invoked automatically, when the object is out of scope. Scope is defined by bracket in C++.
```
int main()
{
	Person p1("tim", "tim", 123);
	{
		Person p2("phong", "phong", 456);
	}
	Person p3("blinn", "blinn", 789);
	return 0;
}
```
The code above would show the Person object p2 will live only inside that bracket. As soon as we reach the closing bracket, p2's destructor will be invoked automatically. And when execution returns 0, the objects that are alive in this scope will be destroyed by their destructors. That would be p1 and p3, because p2 is already out of scope. The p3's destructor will be invoked first, then p1's. The invoking order of destructors is the reverse order of invoking constructors. As we can see, p1 is constructed first, obviously.

// Struct
Struct in C++ is really simple, they are just like class, but if you don't give an accessor when declaring its members, the default accessibility is public, not private as the class.
```
struct Point
{
	int x;
	int y;
};
```
Just like that, we have a struct called Point with two public fields. Many people don't give the accessor for struct, and that's normal. You may wonder wow, that's really easy compared to struct in C#. The reason behind this, is in C++, we always pass by value, unless we explicitly to denote the other way. In C#, we have struct that's passed by value, and class that's passed by reference. Here in C++, the default is to pass by value even for class.

