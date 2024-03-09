

`std::string` has a member function `append()` which adds its argument at the back of the string.
```
string string1{"Hello"};
string1.append(" World"s); // string1 has a value of "Hello World"
```

We have seen the `+=` operator can do the similar thing, that's a result of the history we mentioned. `+=` was developed when `string` class was designed to be a standalone class. Both ways are very similar, but only `+=` can take a char. The `append()` however, can have more arguments:
```
string1.append("wow!!!!s, 3, 2); // "Hello World!!"
```

`insert()` member function adds characters before a specified position. It accepts string literal or a string variable.
```
string string1{"for"};
string1.insert(2, "lde"s); // "folder"

string string2{"care"};
string string3{"omp"};
string2.insert(1, string3); // "compare"
```
`insert()` has a overload to insert a substring.
```
string string1{"xx"};
string string2{"trombone"};
string1.insert(1, string2, 4, 2); // "xbox"
```
`insert()` also has a overload to insert a char for multiple times.
```
string string1("cash");
string1.insert(1, 3, 'r'); // "crrrash"
```
`insert()` can take an iterator.
```
string string1{"ski"};
auto lastPos = end(string1);
string1.insert(lastPos, 2, 'l'); // "skill"
```
When using `insert()` with iterator, we need to watch out for iterator invalidation. Remember iterator is developed around pointer. And `string` underneath is a dynamic array, meaning it would reallocate new memory when it is not big enough. If this happens, the iterator is no long valid, because it is pointing to a memory address no long used by the array.
```
string string1{"orld"};
auto firstPos = begin(string1);
string1.insert(end(string1), 16, 'd'); // cause new memory reallocation
string1.insert(firstPos, 'w'); // firstPos is invalid, and this line will crash
```
The example above is typical, as you may think inserting char at the back of the string will not affect the front of the string, thus not gonna invalidate the front iterator. Well, if there is a memory reallocation, all iterators retrieved before will no longer be valid.