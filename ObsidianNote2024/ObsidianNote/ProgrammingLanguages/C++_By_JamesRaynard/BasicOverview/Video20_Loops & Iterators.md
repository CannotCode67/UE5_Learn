Iterators come in different flavors.

Const iterator prevents modification of the elements in the loop. It is also a container class member.
```
string::const_iterator cit;
for (cit = str.begin(); cit != str.end(); ++cit) {
	cout << *cit;
}
```
In the above example, we can see const iterator can be directly used to compared with regular iterator. Very likely, the const iterator is a derived from regular iterator. And it can be dereferenced to view the content, but not modify it. Container class also have member functions to return a const iterator, `str.cbegin()` and `str.cend()`.
You may want to use const iterator because you don't want to accidentally modify the element, but if the container is const, then you will have to use const iterator for it.

Reverse iterator iterates backwards from the last element, `string::reverse_iterator rit;`. Container classes provide `rbegin()` and `rend()` member functions to return a reverse iterator. `rbegin()` returns a reverse iterator pointing to the last element. In the example of `"Hello"s`, `rbegin()` would point to `"o"`. As we increase the iterator, it moves backwards, so pointing to `"l"`, `"l"`, `"e"`, `"H"`, and the element past `"H"`, respectively. `rend()` returns the iterator pointing to the element past `"H"`. Hope this explains how reverse iterator works.

There are also const reverser iterators, given by `crbegin()` and `crend()` member functions.

After C++14,  we have the global version of all the iterator-returning functions.
```
for (auto it = begin(str); it != end(str); ++it) {
	cout << *it;
}
```
`cbegin(str)`, `cend(str)`, `rbegin(str)`, `rend(str)`, `crbegin(str)`, and `crend(str)`.
We use string as example, but they work with all standard containers as well as built-in arrays.

Ranged for loop is a concise syntax for iterating over containers.
```
for (auto element : vec) {
	cout << element;
}
```
Code above is equivalent to code below.
```
for (auto it = begin(vec); it != end(vec); ++it) {
	int element = *it;
	cout << element;
}
```
We can see that `element` is a copy of the content inside the container, therefore we make changes to `element` will not affect the container content. Whether we use ranged for loop or regular loop.

To affect the content, we know how with a regular loop, but what about ranged for loop?
```
for (auto& element : vec) {
	element += 2;
	cout << element;
}
```
We add qualifier for the `auto`, so that we are referencing the content, hence affect it.