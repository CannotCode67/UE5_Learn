
A stack is just like a queue, except when new elements are added, they are added to the front, not the back like what a queue does. This behavior is called last in, first out, also known as LIFO.
`stack<int> s;`

`std::stack` is defined under the `<stack>` header. It is implemented using deque.
`push()` to add an element to the front.
`pop()` to remove an element from the front.
`top()` returns the top element.
`empty()` returns true if there is no element.
`size()` returns the number of element in the stack.

Stacks are used in C++ runtime to manage the local variables. It is useful to implement the undo functionality.

