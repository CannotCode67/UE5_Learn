To prevent name collisions, C# has a using statement that's simple and clean. Here in C++, all we see is header, so header is equal to using statement right? The answer is yes and no. C# is really a more modern language than C++, and thus C# simplifies things a bit for us. In C++, header, as known as the `#include` statement, tells the complier to copy the code in the included file into the current cpp file. And the namespace is defined by the code in the included file. So even, we already include the file, we still need to type the namespace before the class or function. 
```
#include <iostream>
#include <string>

int main()
{
	std::cout << "Type your name" << "\n";
	std::string name;
	std::cin >> name;
	std::cout << "Hello, " << name << "\n";

	return 0
}
```
Code above, the cout and cin functions are from the standary library written in included iostream file. string class is from the standary library written in included string file. Even though we include both files, we still need to type `std`, the namespace, and `::` the scope resolution operator before we type the function names and class name. Can we not do that? Well, C++ does have a using statement also.
```
#include <iostream>
using std::cout;
using std::cin;
#include <string>
using std:string;
//using namespace std;
int main()
{
	cout << "Type your name" << "\n";
	string name;
	cin >> name;
	cout << "Hello, " << name << "\n";

	return 0
}
```
Code above, with using statement, we can now type the function name or class name directly. But this using statement doesn't work by itslef, it works on top of include statement. In the comment out line of code, we have `using namespace std`, which would bring in all the std functions and classes in those included files into our current cpp file. The advantage of not doing it that way, is we know exactly what we are bring in, and easy to avoid name conflicts. But, it seems okay. As editor can recognize when there is a conflict and we can use a absolute name to deal with that when happens.

```
#include "Person.h"
#include <iostream>

int main()
{
	//...
	retturn 0;
}
```
This example above is showing that when we bring in code we write, that is the class code is in the same file as the main code file, we use a `""` to surround the file name. However, when we bring in code we download, like the standard library, they usually are in a different folder. Thus, we use `<>` to denote that, so the complier would look at the right place to find the code.

Another catch for `#include` statement is remember we say header tells complier to copy the code inside the included file to the current cpp file. Well, if we `#include` the same file more than once, we actually get more than one copy. How?
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

```
#include "Person.h"
#include "Tweeter.h"

int main()
{
	Person person("tim", "blinn", 123);
	Tweeter tweeter("phong", "phong", 456, "@phong");
	return 0;
}
```
Code above, we have tweeter class derived from person class, in order to do that. The header file for tweeter must `#include` the person header file. Then, in the main cpp file, because we need to use person class and tweeter class, it is natural for us to `#include` both headers. Now, when compile, we actually copy the code in Person.h twice, and the build would fail. Don't see that kind of error when we use using statement in C# right. 
There are two ways to prevent this. By the way, the `#` sign is called the perprocessor, anything followed after runs before the compiler. `#include` statement is one of the example. Anthor one is `#pragma once`, which is telling the complier that this file should be copied only once. Where do we put it? In our example, we should put it on top of the Person.h file. That would solve our problem.
In the Person.h file
```
#pragma once
#ifndef _Person_
#define _Person_
#include <string>
class Person
{
	//...
}
#endif // _Person_
```
The code above use `#pragma once` as well as the other method. Those preprocessors mean if not define Person, then define Person, and in the end endif. Nowadays, `#pragma once` is more preferred, but in case we see code like this, we know what it is doing. And preprocessor also exists in C# as well. Remember we see those `#if_editor` things, we should check more on them.

There is also one last alternative to make the code work. That's we don't `#include` the Person.h, because we know Twetter.h already `#include` Person.h. However, that requires whoever writes the ` int main()` to remember a map where each class has `#include` what, which is not practical. Also sometimes it just doesn't work if two classes have to `#include` the same third class to implement their own logics. Therefore, this last alternative is not really an option in reality.



C++20 meaning C++2020, adds a feature called import, which is just the import keyword to replace the header. However, compliers haven't agreed on a standard way to do this, so some compliers require us to write `import <string>;` while some require us to write `import string;`.  You may recongnize this keyword from python. So far let's not use it, but if we see it, we recognize it.

