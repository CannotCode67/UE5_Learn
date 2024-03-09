
`std::multiset` is defined under `<set>` header. `std::multimap` is defined under `<map>` header. They are similar to `set` and `map`, except they allow duplicate keys. Also `multimap` does not support subscripting.
```
multiset<int> mults;
mults.insert(6);
mults.insert(6);
mults.insert(7);
```
Because duplicate keys are allowed, `insert()` for them always succeeds. However, the `erase()` will remove all elements with the same key.
```
multimap<string, int> multim;
multim.insert(make_pair("Jack", 76));
multim.insert(make_pair("Jones", 88));
multim.insert(make_pair("Jack", 55));

multim.erase("Jack"); // both "Jack" elements will be gone
```
If we want to erase only one of them, we can use `erase()` with an iterator to the element we want to get rid of. Luckily, both `multiset` and `multimap` still keep their elements in order, so elements with the same key will be right next to each other. The `find()` will return an iterator to the first element with that key, and `count()` will return the number of elements with that key.
```
multimap<string, int> multim;
multim.insert(make_pair("Jack", 76));
multim.insert(make_pair("Jones", 88));
multim.insert(make_pair("Jack", 55));
multim.insert(make_pair("Jack", 66));
multim.insert(make_pair("Jack", 77));

auto res = multim.find("Jack");
if (res != multim.end()) {
	for (int i = 0; i < multim.count("Jack"); ++i) {
		if (res->second == 66) {
			multim.erase(res);
			break;
		}
		++res;
	}
}
```
With `find()` and `count()`, we get the start iterator and the number for this iterator to reach the end. But there are other member functions such as `lower_bound(key)` and `upper_bound(key)`, working together just like `begin()` and `end()` to give us an iterator range.

`lower_bound(key)` returns an iterator to the first element whose key is equal to `key`, if there is any. If there is none, then it returns an iterator to the first element whose key is just greater than `key`.
`upper_bound(key)` returns an iterator to the first element whose key is just greater than `key`.
```
multimap<string, int> multim;
multim.insert(make_pair("Ben", 76));
multim.insert(make_pair("Ada", 88));
multim.insert(make_pair("Ben", 55));
multim.insert(make_pair("Charles", 66));
multim.insert(make_pair("Dickson", 77));
//{("Ada", 88), ("Ben", 76), ("Ben", 55), ("Charles", 66), ("Dickson", 77)}

auto ben_start = multim.lower_bound("Ben"); // point to ("Ben", 76)
auto ben_end = multim.upper_bound("Ben"); // point to ("Charles", 66)
for (auto it = ben_start; it != ben_end; ++it)
	print(*it);

auto jack_start = multim.lower_bound("Jack"); // point to ("Ada", 88)
auto jack_end = multim.upper_bound("Jack"); // point to ("Ada", 88)
// empty iterator range
```

There is a concise member function `equal_range()` which is equivalent to calling `lower_bound()` followed by `upper_bound()`. It takes in a key value, then returns a `std::pair` object where the first member is the return value from `lower_bound()` and the second member is the return value from `upper_bound()`.
```
auto ben_range = multim.equal_range("Ben");
for (auto it = ben_range.first; it != ben_range.second; ++it)
	print(*it);

// improve readability with structured binding
auto [start, end] = multim.equal_range("Ben");
for (auto it = start; it != end; ++it)
	print(*it);
```