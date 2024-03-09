
A weak pointer is not a smart pointer, it is a safe way to alias a shared pointer.

Pointer aliasing refers to situation where multiple pointers share the same memory address. And it causes troubles easily when traditional pointer is used.
```
int *ptr = new int(36);
int *wptr = ptr;
delete ptr
*wptr; // crash the program
```

A `weak_ptr` is bound to a `shared_ptr`, but it doesn't affect the reference count for it has no access to the shared memory directly. If it needs to access the shared memory, then it has to be converted to the bound `shared_ptr` using its `lock()` member function.
```
auto ptr{make_shared<int>(35)};
weak_ptr<int> wptr = ptr; // bound to a shared pointer
shared_ptr<int> ptr1 = wptr.lock(); // convert operation
```
However, the `lock()` can fail when the shared memory is already released upon the operation. So we should check if we get a valid pointer before accessing it.
```
if (ptr1) {
	...
}
```

Another way to convert a `weak_ptr` into a `shared_ptr` is to use `shared_ptr`'s copy constructor.
`shared_ptr<int> ptr2(wptr);`
This also can fail for the same reason, but when this fails, it throws the `bad_weak_ptr` exception. So, we need to use a try catch block around this operation. I guess the `lock()` member function is more efficient and easier to use.


`weak_ptr` can also be used to handle cyclic references where shared pointers references each other and their reference count will not go down to zero, thus their destructors will never be called.
```
struct Father {
	shared_ptr<Son> mySon;
	void setSon(const shared_ptr<Son>& s) { mySon = s; }
}
struct Son {
	shared_ptr<Father> myFather;
	Son(const shared_ptr<Father>& m): myFather(m) {}
}

int main() {
	auto father = make_shared<Father>();
	auto son = make_shared<Son>(father);
	father->setSon(son);
}
```
`son` is a shared pointer pointing to a memory where a `Son` object lives. This `Son` object has a data member of a shared pointer of `Father`. After the `setSon()`, `father` is a shared pointer to a `Father` object with a data member of a shared pointer of `Son`. So, there are four shared pointers in total. Two live on stack, which are created by constructor calls. The other two live on heap, which are created by copy constructor and copy assignment operator. When the two on stack gets destroyed automatically, we still have the other two living on the heap. That is cyclic reference disabling the destructor calls.

We can avoid this using `weak_ptr`.
```
struct Father {
	shared_ptr<Son> mySon;
	void setSon(const shared_ptr<Son>& s) { mySon = s; }
}
struct Son {
	weak_ptr<Father> myFather; // here is the change
	Son(const shared_ptr<Father>& m): myFather(m) {}
}

int main() {
	auto father = make_shared<Father>();
	auto son = make_shared<Son>(father);
	father->setSon(son);
}
```
Because the `weak_ptr` of Father doesn't affect the counting, so when `father` gets destroyed, it release the memory in which there is a `shared_ptr<Son>` called `mySon`. So `shared_ptr<Son>`'s counting decrements by one. Then when `son` gets destroyed, the counting is zero, resulting the releasing of memory.