Abstract data structures are just there to describe how a certain data structure behaves, but how to implement such behaviours can be ambiguous. On the other hand, using different underlying methods to mimic the behaviours can sometimes cause different algorithmic performances. 

Some data structures are built-in for certain programming languages, but some are not. Because of that, data structures across different programming languages sometimes have different names. For example, in javascript, the default array is a dynamic array (a slightly complex version of array), but in C#, the default array is just the most basic array. Below it's the introduction of array and linked list. They are the most fundemantal data structures which most complex ones are built upon. 

Contiguous data structure is composed of single slabs of memory. 

-In most cases, it means array. 

-array is fixed-size data structure, so once it's initialized, it's size cannot be changed. 

-each element inside array can be efficiently accessed by its index using array[index]. 

-iterating through arrays is faster than iterating through linked list, because element in array is stored successive to each other in memory. 

-each slot stores only the element value, making it very efficient at space usage. 

Linked data structure is composed of distinct chunks of memory bound together by pointers. 

-In most cased, it means linked list. 

-linked list has flexible size. One can keep adding elements into linked list until memory runs out. 

-Each node of linked list contains the element value and pointers. 

-linked list can be singly linked or doubly linked depending on whether one pointer or two are contained by each node. 

-Pointers contained by each node store the reference to the next or the previous node. 

-Example of a doubly linked list below.
![[Algorithm/DataStructureConcepts/GetImage.png]]

-a special node contains the pointer to the head of the structure, here it's doubly linked, so the node also contains the pointer to the tail of the structure. 

-elements cannot be accessed directly through index like array, accessing elements requires scanning through each node until the desired one is found.