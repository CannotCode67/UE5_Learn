
Lambada expression has access to global variables, but not so much for local variables.
```
int global{99};

int main() {
	static int local{42};
	
	[]() {
		cout << global << endl; // this will compile
		cout << local << endl; // this may not compile
	}
}
```
The local variable access is poorly supported by different compilers. For lambda expression to access local variables, some compiler require the local variables to be static, others require the local variables to be const, etc. 

Generally, we don't count on compiler's implementation on this. Instead, the C++ language itself allows lambda expression to take in local variable as copy, which is known as capture.
```
int n{2};
[n] (int arg) { return (n * arg);}

int x{2}, y{3};
[x, y] (int arg) { return (x * arg + y);}
```

Recall when we study functor, we are able to have some flexibility as functor can have data member which will be used as part of the criteria. The lambda with capture underneath is implemented just like that. The compiler will generate a functor class, with data members as the copies of captured variables, which is known as capture by value. The data member is const by default, so we cannot modify it unless we use the `mutable` keyword.
```
vector<string> words {"hello", "happy", "sunny", "weather", "world", "goodbye"};
int n{5}, idx{-1};
auto res = find_if(cbegin(words), cend(words),
                  [n, idx] (const string& str) mutable {++idx; return str.size() > n;});

if (res != cend(words)) {
	cout << idx << endl; // this is still -1
}
```
In the example above, we increment the captured variable `idx` in the lambda expression body. We would have to use `mutable` keyword for it to compile. However, the `idx` variable that lives in the functor object is just a copy. That's why when we output `idx`, we still get a value of `-1`.

To capture by reference, we can add the `&` sign like we do for variables and functions.
```
vector<string> words {"hello", "happy", "sunny", "weather", "world", "goodbye"};
int n{5}, idx{-1};
auto res = find_if(cbegin(words), cend(words),
            [n, &idx] (const string& str) mutable {++idx; return str.size() > n;});

if (res != cend(words)) {
	cout << idx << endl; // this is now 3
}
```

We can make a lambda function capture all variables in scope. This is known as implicit capture.
`[=]` will capture all variables by value.
`[&]` will capture all variables by reference.
We can also make a compound capture.
`[=, &x]` will capture all variables by value, but only x by reference.
`[&, =a, =b]` will capture all variables by reference, but only a and b by value.

Just because we can, doesn't mean we should. The best practice is to capture only variables you need. Use implicit capture sparingly. 

When we put lambda expression in a member function body, all of sudden, lambda can capture `this`, the object itself.
`[this], [&], [=]` are ways to denote capture `this` by reference.
`[*this]` will capture `this` by value, so a copy of the object.
```
class Test {
	int time{10};
public:
	void countdown() {
		[this] () {
			if (time > 0)
				cout << time << endl;
			else if (time == 0)
				cout << "Stop!" << endl;
			--time;
		}(); // this () sign is necessary for using lambda in regular member function
	}
}

int main() {
	Test test;
	for (int i = 0; i < 12; ++i)
		test.countdown();
}
```
But again, why the hell we want to use lambda in regular member function? Maybe we can, but we really shouldn't.

