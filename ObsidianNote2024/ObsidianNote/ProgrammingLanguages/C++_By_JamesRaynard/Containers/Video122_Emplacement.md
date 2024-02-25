
Member functions like `push_back()` and `insert()` requires an existing object because underneath they are copying the existing objects into the container. 
```
vector<book> vec;
book easy_math("Easy Math", "Nike");
vec.insert(begin(vec), easy_math); // create the object before insert
vec.insert(end(vec), book("Hard Math", "Nike")); // create the object during insert
```
With emplacement, we are directly creates the object for container, so save us a copy constructor call and a destructor call. It was introduced in C++11, and we pass the constructor argument value into the member function `emplace()`.
`vec.emplace(begin(vec), "Advanced Math", "Nike");`

There is also `emplace_back()` where element is constructed to put at the back of the container, given the container supports `push_back()`.
`vec.emplace("Advanced Math", "Nike");`

Also `emplace_front()` for containers which support `push_front()`.

For container adapters, there is `emplace()` to work as `push()`.

However, for containers require unique keys, `emplace()` would still create a temporary object to check if the key is unique or not. Also if the implementation uses assignment instead of copy, then a temporary object has to be created. If a type conversion is required during operation, then a temporary object is needed as well.

C++ 17 provides an improved version for maps, `try_emplace()`. It checks for duplicates key before creating the whole object. We know map stores `std::pair` objects. `try_emplace()` would create the key object to run the check, if there is no duplicate, only then it would create the value object to form the `std::pair` object altogether.
It returns a `std::paird` as well like `insert()`.
```
auto [iter, success] = bookmap.try_emplace("author", "title", "publisher");
```
The first argument `"author"`is the key, and the rest is constructor arguments to our custom class of book.

Sometimes, some containers cannot perform copy operation or move operation, then emplacement is the only way to populate those containers. 
But the truth is we rarely encounter containers like that, and compilers nowadays can often optimize away the copy constructor calls, not to mention we also have move semantics to avoid copying. That's why we just use `insert()` and `push_back()`, etc.
Emplacement would had been useful if introduced before C++11.