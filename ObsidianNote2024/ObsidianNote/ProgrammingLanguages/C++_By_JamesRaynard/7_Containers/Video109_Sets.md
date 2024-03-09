
`std::set` is defined under the `<set>` header. Its element consists of only a key, which must be unique because the key is used to identify the element in the container. Imagine an array with two positions indexed the same, which is bad. Also, its elements are const, so once an element is inserted, we cannot change its key.

Remember we said set is an associative container, and associative containers are implemented with tree structure. In fact, set is implemented as a red-black tree. Therefore, the elements are stored in order using `<` operator, with a tree structure.

To add or remove elements in a set, we use `insert()` and `erase()` member functions. If we try to `insert()` an element which is already present in the set, the operation will fail. So, `insert()` returns a `std::pair` object whose second member is a Boolean to tell us whether the operation succeeded or not. The first member is an iterator either to the added element if successful or to the already existed element if unsuccessful. The `erase()` takes in a key value.
```
set<int> set1;
set1.insert(3);
set1.insert(1);
set1.insert(4);

auto res = set1.insert(3);
if (res.second) {
	cout << "operation succeeded" << endl;
}
else {
		cout << "operation failed" << endl;
		set1.erase(3);
}

```

`find()` member function returns an iterator to the element with that given key value. If not found, it returns `end()`. However, we are not allowed to change the key value even there is no other key having the same value, because the elements of a set are const.
```
set<int> set1;
set1.insert(3);
set1.insert(1);
set1.insert(4);

auto res = set.find(3);
if (res != set1.end())
	*res = 11; // this will not compile
```

`count()` member function returns the number of elements with given key value. Since set doesn't allow duplicate values, it returns 1 or 0. But this is an interface standard, it would be useful for map where duplicate values are allowed.

`std::set` is fast at accessing an element. Insertion and deletion are also fast unless rebalance is triggered. We can populate a set with some data to get rid of the duplicate values.