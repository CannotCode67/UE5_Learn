
The standard library provides function objects known as functors. By now, we should know very well what a functor is.

`std::plus` would perform a plus operation.
`std::equal_to` would perform a logical == operation.
etc.

Go to the video for more provided function objects.
But, what is the purpose of them? As far as I can see, they work with the `_if` version of algorithms where we need to provide a predicate. And those provided functors save us the trouble of writing out functors for every possible operators. 