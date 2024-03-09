Iterator is the suggested way to iterate through containers provided in standard library.

Without iterator, how do we iterate through an array?
```
const char str[] = {"T", "h", "i", "s"};
const char* pStart = str; //pointer to the first element of the array
const char* pEnd = str + 4; //pointer to the element after the last element
while (pStart != pEnd) {
	cout << *pStart;
	++pStart;
}
```
We all know `pEnd` is pointing to the null character.

The method above actually works with `std::string` and `std::vector`, because they both are implemented using array. For other containers, we cannot. The universal way to iterate all standard library containers is to use iterator, whether the container is implemented using array or not. The question is never about how iterator is better than pointer. People who design iterator think using pointer is nice and intuitive, and make sure using iterator is similar to that.

```
#include <string>
using namespace std;
int main()
{
	string str{"Hello"};
	string::iterator it = str.begin();
	while (it != str.end()){
		cout << *it;
		++it;
	}
}
```
Iterator is a member of container class, so it is specific to container class.  `std::string` would have a iterator, and `std::vector` also has one, but they are different iterators. Container class often have `begin()` and `end()` to return iterator of that class, pointing to the first element and the element after the last element, just like using pointer. Iterator needs to be dereferenced to get to the content, just like pointer. If there is no content in the container, then `begin()` would equal to `end()`, and that's a check used quite often.


