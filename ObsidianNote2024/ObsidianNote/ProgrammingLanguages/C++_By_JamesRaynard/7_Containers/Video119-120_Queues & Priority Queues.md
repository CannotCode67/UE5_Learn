
A queue is a data structure where elements are stored in the order of their arrival. As a queue is processed, the element at the front is removed and the element behind moves to the front. Elements can only be removed from the front. When new elements are added, they are added at the back. This behavior is called first in, first out, also known as FIFO.

The standard library defines `std::queue` under the `<queue>` header. It is implemented as a deque, for deque is good at operations at the front, although it can be implemented with a vector or list as well. A map with arrival info as key can be a queue as well.

Its interface is very simple, and doesn't support iterators.
`push()` to add an element to the back.
`pop()` to remove an element from the front.
`front()` returns the first element at the front.
`back()` returns the last element at the back.
`empty()` returns true if there is no element.
`size()` returns the number of elements in the queue.

```
int main() {
	queue<int> q;
	q.push(4);
	q.push(3);
	q.push(5);
	q.push(1);
	// {4, 3, 5, 1}
	q.pop();
	// {3, 5, 1}
}
```
Queues are mainly used for temporarily storing data in the order it arrived or happened, a buffer.


Priority queues are queues that use priority as ordering criteria instead of arrival time. C++ provides `std::priority_queue` under the `<queue>` header as well. An element with highest priority will always be place at the front, and processing always takes place with the front element. Lowest priority element will be place at the back, and it will not be processed until all elements with a higher priority are taken care of. Be aware elements with the same priority will be placed right next to each other but the order is random.

`std::priority_deque` can be implemented as a vector or a deque. It has a similar interface.
`push()` to add an element to the queue.
`pop()` to remove the element from the front, so the highest priority element.
`top()` returns the highest priority element.
`empty()` returns true if there is no element.
`size()` returns the number of elements.

If we use our custom class, we need to overload the `<` operator to define priority.