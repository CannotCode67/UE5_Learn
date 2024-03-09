
With regular classes and functions, we declare them in the header file and define them in `cpp` file. On the contrary, when we use templates, we would declare and define the template classes and template functions in the header file. 

We learnt the reason for that is when compiler sees we try to invoke a templated function or create a templated class object, the compiler needs to instantiate a specific version of templated class or templated function for us. To be able to do that, the compiler needs to know the implementation, not just declaration.

If we treat templates the way we treat regular classes and functions, then we put template definition in `cpp` file, and usually, the `cpp` file will `#include` the header file to bring in the declaration, then this `cpp` file will be compiled into object file before the linker put all object files into one executable file. So the compiler will never see the definition until the linker puts them together. By the time linker puts them together, the whole compilation process is way passed the compiler phase.

That's why templates have their definitions written in header files.

Now, clearly the compiler will generate a specific version of templated class or function when we use one. That can happen in different `cpp` files. As the program gets more and more complicated, we will find the same version of templated class or function is generated multiple times across different `cpp` files. When the compilation process moves into the stage of linker, the linker will remove the duplicate definitions, so that there is only one definition of the same version. That sounds perfect, right?

If the program is complicated, and we have many duplicated definitions of template classes or function, then this can greatly increase the resource needed for a compilation, including memory usage, time, etc. This is known as template bloat, which is trivial to small programs. But for some applications whose compilation can take hours or even a day, this template bloat issue can increase the compilation time by 25% or even higher. Now, that is a issue cannot be ignored.

Before C++11, to address this issue, we have to manually instantiate the template.
```
template <typename T>
std::ostream& print(std::ostream& os, const T& t) {
	return os << t;
}

// manual instantiate the string version, seems need to come after template definition
template std::ostream& print(std::ostream& os, const std::string& str);

int main() {
	std::string str{"Hello"};
	print(std::cout, str);
}
```
The programmers need to keep track of what versions are already instantiated and what versions are not, also where to instantiate. That's just not very helpful.

The `extern` keyword can be used to mark variables since the days of C. It is inherited by C++, and it turns the definition of an uninitialized variable into a declaration. It was used to make a global variable accessible across source files.
In source_1.cpp file.
```
extern int someNum; // turned into declaration
someNum = 44;
```
In source_2.cpp file.
```
extern int someNum; // turned into declaration
cout << someNum; // 44
```
There must be exactly one file where the variable is defined. In source_3.cpp file.
```
int someNum; // define someNum but cannot initialize it here
```
So the `someNum` variables in source_1 and source_2 files are the same variable defined in source_3.

In C++11, we can provide manual instantiations in one place, then use `extern` to mark the declaration of those instantiated versions of templated class or function elsewhere.
In the `someTemplate` header file.
```
template <typename T>
std::ostream& print(std::ostream& os, const T& t) {
	return os << t;
}
// this extern marking must come after the template definition
extern template std::ostream& print(std::ostream& os, const std::string& str);

void func(const std::string& str);

template <typename T>
class Test {...}
extern template class Test<int>;
```
In the source_1.cpp file.
```
#include "someTemplate.h"
void func(const std::string& str) {
	print(std::cout, str);
}
```
In the source_2.cpp file.
```
#include "someTemplate.h"
int main() {
	std::string str{"Hello"};
	print(std::cout, str);
	func(str);
}
```
In the source_3.cpp file. The real manual instantiation.
```
#include "someTemplate.h"
template std::ostream& print(std::ostream& os, const std::string& str);

template class Test<int>;
```