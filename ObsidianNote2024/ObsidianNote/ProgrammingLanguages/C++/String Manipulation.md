C# has string cancatenation and string interpoaltion when handling string value. C++ also has those, but syntax is slightly different. Well, for string cancatenation is the same, the difference lays in string interpolation.

// format
In C++, the standary library provides a function called `format`. It is the string interpolation technique of C++. Be aware, this is a feature added in C++ 20.
```
#include "Person.h"
#include <format>
using std::format;
#include <string>
using std::string;

int main()
{
	Person person("Tim", "Blinn", 123);
	string name = format("first name is {}, id is {}}", 
						person.GetName(), person.GetID());
}
```
Code above, is really similar to one of the string interpolation technique C# has, but here in C++, we don't need to give each `{}` a index to signify the order.