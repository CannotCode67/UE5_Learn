
The C++ STL defines a number of functions in `<algorithm>` header.

Typically, an STL algorithm is passed an iterator range to specify which elements in the container will be processed by the algorithm. For example, `begin()` and `end()` would process the entire container.

The algorithm will iterate over this range and call on a function on each element.

The algorithm will return either an iterator presenting a particular element or a value containing the result of some operation on the elements.

Many algorithms call a function on each element which returns bool, this function is known as a predicate, and we are allowed to supply our own predicate. For example, `std::sort()` works by calling the element's `<` operator on each pair of elements.
```
int main(){
	vector<string> names = {"Daddy", "PHB", "Dogbert", "Asok", "Ted"};
	cout << "Vector before sort()\n";
	for (auto name : names)
		cout << name << ",";
	cout << endl << endl;

	sort(begin(names), end(names));

	cout << "Vector after sort()\n";
	for (auto name : names)
		cout << name << ",";
	cout << endl << endl;
}
```
The default behavior of `std::sort()` would call the `string`'s `<` operator, resulting the ascending alphabetical order. But we can provide our own predicate to sort them by word length. 

The `std::sort()` accepts custom predicate by function pointer.
```
// this is a global function
bool is_shorter (const string& lhs, const string& rhs) {
	return lhs.size() < rhs.size();
}

int main() {
	...
	sort(begin(names), end(names), is_shorter);	
}
```
The `std::sort()` also accepts custom predicate by a functor. A functor is just a class object with only the predicate function as its member.
```
class is_shorter {
public:
	// overload () operator
	bool operator() (const string& lhs, const string& rhs) {
		return lhs.size() < rhs.size();
	}
};

int main() {
	...
	//invoke the constructor to create a temporary object
	sort(begin(names), end(names), is_shorter());
}
```