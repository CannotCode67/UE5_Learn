
The universal initialization syntax with `{}`. Being universal means it works for all built-in types and types defined in the standard library.
```
int x{7}; // int x = 7;

string s{"Hello World"}; // string s("Hello World");

vector<int> vec{4, 2, 3, 5, 1};
// vector<int> vec;
// vec.push_back(4);
// vec.push_back(2);
//...
```

No trucation
```
int x = 7.7; // x has a value of 7
int x{7.7}; // throws a compiler error
```

More intuitive
```
vector<int> old_way(4); // vector with elements 0, 0, 0, 0
vector<int> old_way(4, 2); // vector with elements 2, 2, 2, 2
vector<int> new_way{4}; // vector with element 4
vector<int> new_way{4, 2}; // vector with element 4, 2
```

No ambiguity
```
Test test(); // can be function declaration or object creation?
Test test{}; // can only be object creation
```

A vector that holds elements which are vectors, like below.
```
vector<vector<int>> VecOfVecOfInt;
```
To improve the readability, we have two ways:
```
//typedef vector<int> IntVec;
using IntVec = vector<int>;

int main()
{
	vector<IntVec> VecOfVecOfInt;
}
```
Cannot do both, that's why one of them is commented out.

For the null pointer, the old one NULL has a value of 0, but its type is not defined, so different compilers may implement differently. And people use NULL to represent value 0, hacky. The new one nullptr has a special type defined, and this type is compatible with any pointer type. But it is not compatible with integer, so it cannot be used to represent value 0, which is a good thing. 
```
func(nullptr); // func(int*) with most compilers
func(NULL); // func(int) with VC++ compiler, func(int*) with CLang compiler
// gcc doesn't compile.
```
Example
```
using namespace std;

void func(int i)
{
	cout << "func(int i) is called\n";
}

void func(int* i)
{
	cout << "func(int* i) is called\n";
}

int main()
{
	func(NULL);
	func(nullptr);
}
```
The output would show the first one calls the `func(int i)` and the second one calls `func(int* i)`.