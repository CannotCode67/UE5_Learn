What is a binary search tree? Before that, what is a tree? We mention tree structure when talking about heap. Here we explain this abstraction a little bit more. A tree is an undirected graph which satisfies these properties: an acyclic (without cycle) connected graph, a connected graph with N nodes and N-1 edges, an graph in which any two vertices are connected by exactly one path. 

A rooted tree means we have a reference to the root node of the tree. Which node to be the root varies from different standards. A child node is a node extending from another node which is called a parent node. Root node has no parent node, although it may be useful to assign the parent pointer to the root node itself. A leaf node is a node without any children. 

Now we have some basic understanding of tree structure, let's proceed further towards our BST. One step further would be a binary tree, which is a tree for which every node has at most two child nodes. Finally, a binary search tree is a binary tree that satisfies the BST invariant: left subtree has smaller elements and right subtree has larger elements. The next question would be what about the equivalent value. It depends on whether you want to allow duplicate values in your tree. BST operations allow for duplicate values, but most of the time we are only interested in having unique elements inside our tree. Not just number, any comparable data can be stored in BST. 

BST is the template for many more complex trees. As it supports fast search and flexible update.
![[Algorithm/BinarySearchTree/GetImage.png]]

The main reason why it performs differently is BST can get unbalanced like below.
![[Algorithm/BinarySearchTree/GetImage.png]]

Balanced BST has insertion/deletion procedure that adjusts the tree a little after each insertion/deletion, keeping it close enough to be balanced so the maximum height is logarithmic. Red-black tree and splay tree are examples of balanced BST. 

Removing a parent node with two child nodes in BST means there are two possible successors: one is the biggest value in the left subtree, another one is the smallest value in the right subtree. Pick either one is fine.