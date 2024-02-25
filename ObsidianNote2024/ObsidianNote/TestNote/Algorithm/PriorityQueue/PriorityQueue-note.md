Priority queue is an abstract data structure where the dequeued element has the maximum or minimum value inside the structure no matter if the elements arrive in such a order or not. One might argue that sorting algorithm can also do the trick. As matter of fact, it certainly can, but re-sorting everything when there is an arrival of element is not as efficient as using the priority queue. The secret of priority queue is an actual data structure called heap, which is array with some twist to implement the behaviours priority queue needs. That is not equal to say priority queue is heap, because we can still use other data structures to implement priority queue. It's just heap happens to be the perfect data structure to implement priority queue behaviour with the greatest algorithmic performance in most cases. 

There are all kinds of heaps such as minimum heap, maximum heap, interval heap, etc. Using different heaps can implement different priority queues, like maximum heap are used to implement maximum priority queue. Luckily, they all share some common charatacteristics. 

We usually use a tree to describe the heap hierarchy. It is easy to confuse heap strcutrue with binary search tree, whch is totally different data structure. We use min-heap to illustrate the heap invariant. 

Heap invariant in mim heap: If A is a parent node, then the value of A is always less than or equal to the value of its child nodes, given there are child nodes. 

Does it have to be binary? No. 

Does it have to be tree? Yes. 

Does it have to be balanced? No. 

Q&A above is to demystify properties of a heap. However, the heap we use for priority queue usually is binary and balanced like the one below.
![[Algorithm/PriorityQueue/GetImage (1).png]]

And the indexes for a parent node and its children node is calculated as above. Root node is always in index 1 position, so accessing the minimum value takes constant time like accessing any other index position in array. Index zero position is empty for it cannot be used to calculate children nodes' positions.