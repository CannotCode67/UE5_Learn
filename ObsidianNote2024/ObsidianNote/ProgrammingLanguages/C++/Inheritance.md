Inheritance is referring we have a class which is derived from a base class. That's the same in C++. What's not the same is the way we do it. 

In the header file, we need to specify a base class after `:` just like C#, but we see we also need to write a `public` accessor before the base class name. That's C++.
Header file:
```
#include "Person.h"

class Tweeter: public Person
{
	private:
		std::string twitterhandle;
		
	public:
		Tweeter(std::string first, std::string last, int num, std::string handle)
		~Tweeter();
}
```
Implementation file:
```
#include "Tweeter.h"
#include <string>
using std::sting;
#include <iostream>
using std::cout;

Tweeter::Tweeter(string first, string last, int num, string handle)
	: Person(first, last, num), twitterhandle(handle)
{
	cout << "constructing tweeter" << twitterhandle << "\n";
}

Tweeter::~Tweeter(void)
{
	cout << "destructing tweeter" << twitterhandle << "\n";
}
```
Code above, we see the technique again, directly pass the values into the constructors to speed up, but this time, we see this technique also works with the base class constructor as well. I don't know why we need to give the destructor a void parameter here though.

// constructor and destructor invoking order
When we create a tweeter object with `Tweeter t1("tim", "blinn", 123, "@phong");`. The base class constructor is invoked first, that is the Person class constructor in this case. Then, the Tweeter constructor is invoked. When the tweeter object goes out of scope, it is the reverse order. So the tweeter destructor is invoked first, then the person class destructor is invoked. Why's that important? I don't know, maybe that's something we should know when writing desctructor to clean up resources.

//