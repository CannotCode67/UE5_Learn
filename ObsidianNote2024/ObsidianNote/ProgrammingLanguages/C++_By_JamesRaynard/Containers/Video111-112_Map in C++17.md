
In the study of map, we learnt that the subscripting or `[]` operator of the map will assign a new value to an existed element or create a new element, then assign the value.
```
map<string, int> scores;
scores.insert(make_pair("Jack", 86));
scores.insert({"Jones", 88});

scores["Jack"] = 66; // will replace 86 with 66
scores["Jacky"] = 70; // will insert an element with "Jacky" as the key with value default initialized
// then assign 70 as value
```
The first problem is when a new element is created, its value is default initialized, meaning the value type must be built-in type or has a default constructor. Otherwise, it cannot compile.
The second problem is we have no ways to tell which outcome took place. The `[]` operator returns a reference to the element's value member, so it doesn't help.

The safe way to go about this is to use the `insert()` member function. Its return `std::pair` object also helps us to know what happens.
```
auto res = scores.insert(make_pair("Jacky", 70));
if (!res.second) {
	cout << "Jacky is already existed" << endl;
	res.first->second = 70; 
}
```
As you might notice, the readability is a little bit bad since we are accessing two different `std::pair` objects each with their first and second members having different data. 

C++17 introduced something called structured binding which might improve this. The syntax declares multiple local variables, and bind them with values from a compound data structure. The compiler is up to deduce the type.
```
map<string, int> scores;
scores.insert(make_pair("Jack", 86));
scores.insert({"Jones", 88});
// structured binding below
for (auto [name, score] : scores) {
	cout << name << " scores " << score << endl;
}
```
Now, we use this structured binding with our `insert()` member function.
```
auto [iter, success] = scores.insert(make_pair("Jacky"), 55);
if (!success) {
	auto [name, score] = *iter;
	cout << name << " already exists" << endl;
	score = 70; // this only changes the local variable which is a copy
}
```
Well, structured binding does improve the readability, but those local variables are copies. If we want to assign value to existed element, they would not help.

C++17 provides yet another map member function `insert_or_assign()`. It takes a key and a value, and returns a `std::pair` object. If we end up inserting some new elements, then this returned `std::pair` object would have its first member be the iterator to the new element, and its second member be true. If we end up assigning value, then this pair object would have its first member be the iterator to the existed element, and its second member be false. Basically, this is the improved version of `insert()`.

C++17 allows initializer in the if statement condition, so we can do something crazy like this.
```
if (auto [ele, suc] = socres.insert_or_assign("Jacky", 55); suc) {
	cout << "we insert a new element" << endl;
}
else {
	cout << "we assign a new value" << endl;
}
```

