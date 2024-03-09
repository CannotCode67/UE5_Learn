
The standard library defines a class called `std::pair` under the `<utility>` header. It is used as a container for storing two pieces of data which can be of different types. With this, a function can return two data items instead of one. But you may argue writing a struct is not that difficult. And you are right, the biggest reason to use `std::pair` is some containers in standard library use it as element, such as dictionary. If we don't deal with those containers, then we are free to write any struct or class to store our data.

`std::pair` is templated type, so we should specify the types of both members.
`pair<string, string> wordpair {"hello"s, "there"s};`
Then, we can access its members by first and second.
`wordpair.first; //"hello"`
`wordpair.second; //"there"`

We can call `make_pair()` to create a pair variable as well.
`auto wordpair = make_pair("hello"s, "there"s);`

C++17 offers feature called CTAD where we don't need to specify the types, and let the compiler to deduce them. Whether to use it is up to you, but I prefer writing out the types.
`pair wordpair {"hello"s, "there"s};`

