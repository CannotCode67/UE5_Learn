
A `lvalue` reference is a refence to a `lvalue`. It is the reference we study before. Reference must be initialized upon declaration. Its initialized value must be a memory address value. Once it is assigned, we cannot change the memory address, all we can change is the object pointed by that memory address.
```
int x;
int& y = x; // equivalent to int* y = &x;
y = 3; // equivalent to *y = 3;
```
So, `x` must be a `lvalue` as we need to take its memory address. That is also why we cannot do `int& y = 3;` because `&3` is not legal and it will not compile. Unless, we use const reference, `const int& y = 3;`, where the compiler adds extra code to make it work.

Next, what is a `rvalue` reference? It is a type with more restriction upon `rvalue`. If an object is a `rvalue` reference, then it must be a `rvalue` while being moveable.  If a function takes `rvalue` reference, then it is equal to say this function uses move semantics only, if an object doesn't allow move semantics to happen, then it will not be accepted by this function even though it is a `rvalue`. The syntax we use to indicate that is `&&`.
```
void func(int&& x); // func only accepts rvalue of int, built-in types are moveable

func(2); // this will compile
int y{2};
func(y); // this will not compile
```
When we indicate a function only accepts `rvalue`, we say its argument is a `rvalue` reference. The passed object must be a `rvalue`, and its type is moveable, otherwise it will not compile. When passed object meets those requirements, it will be moved into argument instead of being copied.

If we want to pass a `lvalue` to such a function, we can cast it to `rvalue` with `std::move()`.
```
int y{2};
func(std::move(y)); // this will compile
```
After this operation, `y`'s data is either lost or corrupted. We should only access its data after we reassign some values.

Now, we have a function taking a regular reference or `lvalue` reference, and an overload taking a `rvalue` reference.
```
void test(const string& s) {
	cout << "lvalue reference version" << endl;
}
void test(string&& s) {
	cout << "rvalue reference version" << endl;
}
```

We pass some objects to see which one is invoked to tell which object is `rvalue` and which is not.
```
string l{"Perm"};
string& ref_l = l;
test(string{"Temp"}); // rvalue reference version
test(l); // lvalue reference version
test(ref_l); // lvalue reference version
string&& r{"Semp"};
test(r);  // lvalue reference version
test(std::move(r)); // rvalue reference version
```
