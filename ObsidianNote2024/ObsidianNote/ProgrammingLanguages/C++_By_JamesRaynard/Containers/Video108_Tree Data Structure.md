
C++ associative containers are implemented using a tree structure. Inside a tree, each element has its own node. A node consists of the value and two pointers, one point to left, and one point to right. Left pointer will take us to a node with a smaller value, and right pointer will take us to a node with a bigger value.

Adding and removing elements are very fast. Finding an element is also very fast.

Inserting and erasing elements may cause the tree to become unbalanced. When this happens, we need to choose a new root and moving elements around until the tree is balanced, which is known as rebalance the tree.

Some tree structures are automatically rebalanced, such as red-black tree and AVL tree.