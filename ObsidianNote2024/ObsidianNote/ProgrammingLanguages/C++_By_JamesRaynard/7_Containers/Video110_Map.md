
`std::map` is defined under the `<map>` header. Its element consists of a `std::pair` object where the first member is the key, and the second member is the value. So we must provide two template arguments, one for key type, the other for value type. Like set, each element must have a unique key, but the values can be the same.

`std::map` is implemented as a red-black tree also. Elements are stored in order using `<` operator, with a tree structure.

To add or remove element, we use the `insert()` and `erase()` member functions. `insert()` takes a `std::pair` object, `m.insert(make_pair(key, value));` And it returns a `std::pair` object just like the set's version. `erase()` takes in a key.
```
map<string, int> scores;
scores.insert(make_pair("Jack", 86));
scores.insert({"Jones", 88}); // universal initialize syntax also works

auto res = scores.insert({"Jack", 60});
if (res.second) {
	cout << "operation succeeded" << endl;
}
else {
	cout << "operation failed" << endl;
	scores.erase("Jack");
}
```

Unlike list and set, map supports subscripting, but it is slightly different from how it works in vector and array. We use number to index vector and array, but with map we use the key.
```
scores["Jack"] // will get 86, the value of the element
scores["Jack"] = 66; // will replace 86 with 66
scores["Jacky"] = 70; // will insert an element with "Jacky" as the key with value default initialized
// then assign 70 as value
```
As the example above shows, we are allowed to change the element's value. However, we are still not allowed to change the element's key.
```
auto jack = scores.find("Jack");
if (jack != scores.end()) {
	jack->second = 55; // change value is ok
	jack->first = "ace" // cannot compile, not allowed to change key
}
```


`std::map` is fast at accessing element. Insertion and deletion are fast unless rebalance is triggered. But it can store a pair of data, which makes it more useful than set in many cases.