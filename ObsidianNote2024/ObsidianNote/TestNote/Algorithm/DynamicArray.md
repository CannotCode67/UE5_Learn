Array's characteristics of fixed size limits its practicality, so dynamic array is invented to specifically overcome this issue. It use static array as the underlying structure, with a few mechanism added to have additional behaviour over merely static array. 

Dynamic array can automatically increase the array size behind the scene. When the underlying static array is full and a new element is added, it creates a new static array with more space.  Original elements are copied from the previous array before it gets deleted, then the new element is added directly to the new static array. 

That leads to a question: how big should the new array be? 

In theory, the growth factor can be any number bigger than one. In most cases, it would be in the interval of (1, 2]. This growth factor affects the frequency of creating new static array. Idealy, it should happen as less as possible. However, there is also space factor taken into consideration, otherwise creating a huge static array can avoid any of this, including the need of  a dynamic array. Therefore, a growth factor in (1, 2] should most of the time, balance the space factor and the frequency of creating new static array. 

In fact, the cost of inceasing array size would be even out in amortized analysis, because the increased portion of the new array size will be bigger and bigger, yet the input size is asummed to increase at a constant rate. Therefore, creating new array would happen gradually less and less as the input grows. 

Now we know dynamic array increases the array size when needed, but does it shrink the array size when elements are removed from it? It can, if we choose to implement this behaviour. Otherwise, it is treated just as a static array for removing elements and searching elements.