
A callable object is something supports the `()` operator. So far we have learnt a several different types of callable objects. They are function pointer, functor, lambda expression, and objects returned from `bind()`. Here the function pointer refers to pointer to non-member function, as pointer to member function is quite different.

Now, function pointer and a functors both have the same arguments and return type, so their function signatures are the same. We might expect they are callable objects of the same type where the type is deduced by the compiler. But the fact is compiler will give them different types even they share the same function signature.

When we have a function takes a callable object, or a container to store a callable object, we normally want them to accept any callable object whether it is a functor or a lambda expression. But they all have different types, how can we do that?

C++11 provides `std::function` under the `<functional>` header. This class has a private member to store a callable object. It is a templated class where its template parameter is the callable object's function signature.

Here, the `function` object accepts any callable object with a function signature of const string reference as argument and bool as return type. One more weird syntax for C++.
```
std::function<bool(const string&)> match_ptr;
```

`std::function` performs what is called type erasure. It means the callable object's original type is lost and cannot be recovered. So we have no way to find out the stored callable object is a functor or a lambda or anything else.

Here, we try to write our own `count_if`, but it only works with vector of string.
```
int count_strings(vector<string>& texts, std::function<bool(const string&)> match_ptr){
	int counter = 0;
	for (auto text : texts) {
		if (match_ptr(text)) {
			++counter;
		}
	}
	return counter;
}
```
We have all kinds of callable objects.
```
bool match(const string& test) {
	return test == "cat";
}

class functor_match {
public:
	bool operator() (const string& test) {
		return test == "cat";
	}
}

bool bind_match(const string& animal_1, const string& animal_2) {
	return animal_1 == animal_2;
}
```
In the main(), we have:
```
int main() {
	int n;
	vector<string> animals = {"cat", "dog", "tiger", "lion", "cat", "bear"};
	auto bind_match_cat = std::bind(bind_match, _1, "cat");
	n = count_strings(animals, &match); // not sure if & is needed or not
	n = count_strings(animals, functor_match());
	n = count_strings(animals, [](const string& test){return test == "cat";});
	n = count_strings(animals, bind_match_cat);
}
```
n will be 2 for all of them.