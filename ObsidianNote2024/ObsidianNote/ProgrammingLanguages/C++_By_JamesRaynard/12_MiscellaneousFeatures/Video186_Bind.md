
`std::bind()` is a function introduced in C++11 under `<functional>` header. To understand what it does, let's start from an example without it.
```
bool match(const string& animal) {
	return animal == "cat";
}

int main() {
	vector<string> animals = {"cat", "dog", "tiger", "lion", "cat", "bear"};
	auto n = count_if(cbegin(animals), cend(animals), match); // n is 2
}
```
Now, what if we want the `match()` to be more generic?
```
bool match(const string& animal_1, const string& animal_2) {
	return animal_1 == animal_2;
}
```
Can we pass the second argument only? The answer is no.
```
int main() {
	vector<string> animals = {"cat", "dog", "tiger", "lion", "cat", "bear"};
	auto n = count_if(cbegin(animals), cend(animals), match("cat")); // not compile
}
```
The compiler will think we are passing `"cat"` to `match()` as the first argument, and `count_if()` will can only feed the first argument which is already fed.

With `bind()`, we can perform a partial evaluation. Its first argument is a callable object. And any additional argument it take will be passed into the callable object in the order of the argument list.
```
auto match_cat = bind(match, "cat");
match_cat(); // is called as match("cat")
```
But our generic `match()` takes two arguments right? We can forward the arguments, by there is no guarantee for its ordering. 
```
auto match_cat = bind(match, "cat");
match_cat("dog"); // is called as match("dog", "cat") or match("cat", "dog")
```
There are placeholder arguments defined in the `std::placeholders` namespace. They are something like `_1`, `_2`, `_3`,etc. The number indicates it is holding the position for which argument of this bind function. So `_1` will be replaced by the first argument of `match_cat()`.
```
auto match_cat = bind(match, _1, "cat");
match_cat("dog"); // is called as match("dog", "cat") for sure
```
Remember `bind()`'s arguments after the first one will be passed into the callable object in the same order.
```
auto match_cat = bind(match, _2, "cat", _1);
match_cat(arg1, arg2); // is called as match(arg2, "cat", arg1)
```

Now we have the solution to our example in the beginning. But partial evaluation is something we study before when we learn lambda expression.
```
bool match(const string& animal_1, const string& animal_2) {
	return animal_1 == animal_2;
}

int main() {
	auto match_cat = bind(match, _1, "cat");
	auto match_cat = [animal_2 = "cat"](const string& animal_1){
					return animal_1 == animal_2; }
}
```

So, the whole story is `bind()` was introduced in C++11 to solve the problem we have in the beginning. Then, designer for C++ realize lambda expression is capable of doing the same thing, so they add the local variable initialization to lambda expression in C++14. Both ways are valid. Personally, I think lambda expression is easier to use and clearer for reading.

C++98 provided `bind1st()` and `bind2nd()` which will bind only the first argument or the second argument respectively. They were clearly not flexible, and were removed in C++17.