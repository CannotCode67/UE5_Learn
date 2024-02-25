
Iterator arithmetic is similar to ones with pointer.

Adding to an iterator moves it towards the back of the container.
`auto second = begin(str) + 1;`

Subtracting from an iterator moves it towards the front of the container.
`auto last = end(str) - 1;`

`end() - begin()` gives the number of elements.
`auto mid = begin(str) + (end(str) - begin(str))/2;`

There are functions for these.
`next()` takes an iterator and returns the following iterator.
`auto second = next(begin(str));`
`prev()` takes an iterator and returns the previous iterator.
`auto last = prev(end(str));`
`distance()` returns the number of steps needed to go from first argument to the second.
`distance(begin(str), end(str)); //returns the number of elements`
`advance()` moves an iterator forward by its second argument.
```
auto mid = begin(str);
advance(mid, distance(begin(str), end(str))/2);
```

Iterator ranges are half-open ranges. Iterator must >= begin() and < end().

