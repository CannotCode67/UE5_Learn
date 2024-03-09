
We know that associative containers store elements in an order which depends on the elements' keys, so by default they are ordered. C++11 introduced unordered versions of associative containers, which are implemented using a hash table instead of a tree.

Following is an explanation of hash tables.
A hash table is an array of buckets. This bucket can be some sequential containers like list or vector. A hash function is performed to determine which bucket an element should go to. The return of a hash function is called hash number which is also the index into the array. Accessing elements and insertions would need to use the hash function to get the hash number first, then perform corresponding container operation on that bucket. 
Apparently, the hash function here determines the quality of the hash table. For maximum efficiency, each element should have its own bucket, which is known perfect hashing. In reality, most hash functions have hash collisions where different keys result in the same hash number. When hash collisions happen, we have multiple elements in one bucket, which is fine as long as there are not too many elements inside one bucket. Just like too many nodes on one tree branch would trigger rebalance of the tree, hash table would resize its bucket if too many elements end up in one bucket.

The unordered associative containers in C++ store pointers to elements, not elements themselves, but this is an implementation detail. The way we use them as if they store the elements, not the pointers to elements.
```
unordered_map<string, int> scores;
scores.insert({"Ben", 88});
scores.insert({"Ada", 95});
scores.insert({"Charles", 80});
scores.insert({"Ben", 78});
scores.insert({"Dickson", 56});
// no duplicate key for map, so the ("Ben", 78) will has no effect
```
The order of elements is random, not something we can rely on.

We have `unordered_set`, `unordered_multiset`, `unordered_map`, and `unordered_multimap`. Operations on unordered associative containers are usually faster than their sorted equivalents. However, the resize of hash table is just as slow as the rebalance of tree if not slower. They support `insert()`, `find()`, `count()` and `erase()`, but allow only forward iteration. The unordered versions of `multiset` and `multimap` do not support `lower_bound()` and `upper_bound()`, but still allow `equal_range()`.

A common practice is to perform operations on unordered versions, then copy the data into a sorted one for presentation.
`copy(begin(unsorted), end(unsorted), inserter(sorted, end(sorted)));`
